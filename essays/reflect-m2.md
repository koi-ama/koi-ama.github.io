---
layout: essay
type: essay
title: "Patterns in the Pantry: Rediscovering Our Club Oven Lovin’ Architecture After M2"
date: 2025-12-03
published: true
tags:
  - design patterns
  - software engineering
  - final project
  - react
  - prisma
---

## From Thanksgiving Layover to Design Pattern Do-Over

Milestone 2 (M2) went by in a blur. I spent that stretch bouncing between Boston job interviews and Thanksgiving travel, which meant I wasn’t as present in the repo as I wanted to be. Coming back now feels like walking into a kitchen after service: the dishes are stacked, the ingredients are half-prepped, and the aroma tells me a lot about what actually happened while I was gone.

This essay is my way of catching uplooking at our Club Oven Lovin’ codebase through “design pattern glasses” to understand what we built, where patterns already peek through, and how a more deliberate pattern mindset could fuel Milestone 3 (M3).

## Patterns Are Recipes for Repeatable Success

Design patterns are to software what recipes are to cooking: they capture proven arrangements of ingredients and steps so teams don’t have to reinvent the dish every time. More importantly, they give us a shared vocabulary. Saying “let’s use a repository here” or “this looks like a container component” lets teammates coordinate quickly, just like cooks calling “mise en place” or “fire two risottos.”

Patterns are not rigid templates; they’re heuristics that balance structure with flexibility. Used well, they prevent the project from devolving into ad hoc decisions that don’t scale. In the context of our final project, patterns are the difference between a one-off prototype and a codebase that can survive new features, new teammates, and new bugs.

## The M2 Menu We Promised

Officially, M2 came with two big promises: richer functionality and a stronger software engineering process.

On the **functional** side, we committed to:

- Providing all the functionality desired for M1, plus significant additional functionality.
- Deploying at least **four pages beyond the landing page** (not counting the Sign In/Register template pages).
- Having at least one page that **reads data from PostgreSQL**.
- Having at least one page that **writes data to PostgreSQL via a form**.

On the **process** side, we were supposed to:

- Use GitHub issues and a GitHub Project called **“M2”** to manage development.
- Follow **Issue Driven Project Management (IDPM)** practices.
- Update our GitHub Pages project home with an Overview, User Guide, Developer Guide, and links to deployment and project boards.
- Name all branches according to IDPM guidelines.
- Implement **Playwright “availability” tests** for all pages and forms.
- Run **ESLint and Playwright** on every commit to the master branch via GitHub Actions.
- Display a CI status badge on the project home page.
- Link to both the **M2** and **M3** project boards, with M2 issues done and M3 issues ready to go.

Hidden inside this requirements list is a whole cookbook of patterns: routing and page modularity (architectural patterns), form-handling and validation (strategy/template patterns), data-access consistency (repository pattern), and testing/CI (deployment pipeline and process patterns).

## What Actually Hit Production (and How It’s Structured)

Opening the repo now, I see a Next.js app with a layered feel:

- **Root shell as layout pattern.**  
  `src/app/layout.tsx` wraps every page with a consistent navbar, footer, and providers. This is the classic _Layout_ pattern: a stable frame that concentrates cross-cutting UI concerns in one place.

- **Context façade for authentication.**  
  `src/app/providers.tsx` pipes everything through `SessionProvider`, giving child components access to session state without manual prop drilling. It behaves like a small _Facade_ combined with an _Observer_-like mechanism: components subscribe to session updates without knowing the auth details.

- **Data access consolidated in server actions.**  
  `src/lib/dbActions.ts` centralizes Prisma writes (add/edit/delete) and redirects. It resembles a _Repository_ pattern: instead of sprinkling DB calls across pages, we funnel them through a few well-known functions.

- **Form logic encapsulated with validation.**  
  Components like `src/components/AddRecipeForm.tsx` use `react-hook-form` and `yupResolver` to manage state and validation, acting like a _Template Method_: the library manages the lifecycle while we plug in field-level specifics.

- **Page protection helpers.**  
  `src/lib/page-protection.ts` provides helpers such as `loggedInProtectedPage` and `adminProtectedPage`, giving us a reusable guarda small _Navigation Guard_ patternthat keeps authorization logic consistent across pages.

- **UI broken into presentational cards.**  
  Components such as `RecipeCard`, `KitchenTipsCard`, and `Vendor` focus on rendering, while pages like `browse-recipes` orchestrate data fetching and layout. That’s the classic _Container/Presentational_ pattern, even if we don’t always separate them perfectly.

To make this less abstract, here’s what our repository-like consolidation looks like in practice:

```ts
// src/lib/dbActions.ts
export async function addRecipe(recipe: {
  name: string;
  image: string;
  ingredients: string;
  steps: string;
  tags: string;
  dietaryRestrictions: string[];
  owner: string;
}) {
  await prisma.recipe.create({
    data: {
      name: recipe.name,
      image: recipe.image,
      ingredients: recipe.ingredients,
      steps: recipe.steps,
      tags: recipe.tags.split(",").map((t) => t.trim()),
      dietaryRestrictions: recipe.dietaryRestrictions,
      owner: recipe.owner,
    },
  });

  redirect("/browse-recipes");
}
```

All write logic for recipes is in one place; pages call this without caring about Prisma details. That’s exactly the value of a Repository: we could swap Prisma later or add logging without touching the call sites.

Form handling shows a Template/Strategy flavor. In `AddRecipeForm`, `react-hook-form` owns the workflowvalidation, submission, error collectionwhile we configure it with a schema and field registrations:

```ts
// src/components/AddRecipeForm.tsx
const {
  register,
  handleSubmit,
  formState: { errors },
  reset,
} = useForm<AddRecipeFormData>({
  resolver: yupResolver(AddRecipeSchema),
});

return (
  <Form onSubmit={handleSubmit(onSubmit)} data-testid="add-recipe-form">
    <Form.Control type="text" {...register("name")} isInvalid={!!errors.name} />
    <Form.Control.Feedback type="invalid">
      {errors.name?.message}
    </Form.Control.Feedback>
    {/* ...other fields... */}
  </Form>
);
```

The hook provides the algorithmic skeleton; we supply strategies (validators) and concrete fields. That aligns with the “don’t repeat form boilerplate” ethos.

Authorization shows a mini guard pattern. `adminProtectedPage` composes `loggedInProtectedPage` and a role check to redirect users appropriately. Instead of scattering `if (!session) redirect()` checks in every page, we call a shared guard. It’s simple, but it’s a recognizable pattern.

Here’s an example of the UI this architecture supports:

![Browse recipes page showing Club Oven Lovin’ recipe cards with images, tags, and dietary badges](/images/browse-recipes.png)

Even this screenshot reflects patterns: container pages fetch data and pass it into smaller, reusable cards that know nothing about Prisma or routing.

## Hidden Patterns in Our M2 Delivery

Connecting the dots:

- **Layout + Providers → Facade & Observer.**  
  The layout injects `SessionProvider`, meaning components listen to session changes without manual wiring. That lowers coupling and supports Playwright availability tests, since the auth surface is consistent.

- **Repository-like `dbActions.ts` → Single entry for data writes.**  
  This supports the M2 requirement that “at least one page writes to PostgreSQL via a form.” We meet it through `addRecipe` and similar actions while keeping write rules centralized.

- **Container/Presentational split → Easier testing.**  
  Playwright availability tests run against pages, but presentational components like `RecipeCard` are easily inspectable in isolation. Even though we haven’t written unit tests for them yet, the structure invites it.

- **Navigation guards → Consistent routing behavior.**  
  Guards help the “four deployed pages” goal by ensuring protected pages behave uniformly. It’s easier to reason about availability tests when guard behavior is predictable.

- **Process patterns → CI and IDPM.**  
  GitHub Actions running ESLint and Playwright on each master commit mirror the **Deployment Pipeline** patterna repeatable recipe for quality. The M2 project board and IDPM branch naming function like patterns of collaboration: predictable issue flow replaces one-off decisions.

In other words, even though we didn’t explicitly shout “we’re using the Repository pattern!” during M2, our code still grew into shapes that resemble known design patterns.

## If I Had Been Fully There for M2…

Being partially absent meant I missed chances to standardize our patterns. Looking now, there are several places where I would push further if I had been fully present:

- **A clearer Container/Presentational boundary.**  
  Some page files mix data fetching, business rules, and UI markup. I’d extract data loaders into page-level containers and keep cards purely presentational. That separation would simplify adding M3 features (for example, pagination or filters), because data changes wouldn’t ripple through JSX markup.

- **A formal Repository layer for reads as well as writes.**  
  We centralized writes in `dbActions.ts`, but reads are scattered across pages and API routes. Creating `recipeRepository` and `stuffRepository` modules would let us add caching, error telemetry, or role-based filtering in one placeboosting both readability and testability.

- **Reusable form hooks.**  
  `AddRecipeForm`, `EditStuffForm`, and `EditUserProfileForm` each handle submission, loading, and errors slightly differently. A custom hook like `useFormSubmission({ schema, onSubmit })` could enforce consistent loading states and error toasts. That’s a Template Method/Strategy combo waiting to happen.

- **Error and loading state pattern.**  
  Some pages show a `LoadingSpinner`, others redirect silently. A shared wrapper or hook (for example, `withAsyncState`) would normalize user experience and reduce duplication.

- **Navigation guard composability.**  
  Right now guards are functions that redirect immediately. Wrapping them in a higher-order component or middleware-like pipeline would make role-based rules declarative and easier to extend for M3 (for example, vendor-only pages).

- **Domain-focused folder structure.**  
  Grouping components, pages, and repositories by domain (recipes, vendors, user profiles) would reinforce pattern boundaries and make issue creation under IDPM more discoverable.

Each of these changes would have made M2 smoother: Playwright tests benefit from consistent loading/error UX, CI becomes less flaky when data access is centralized, and pull request reviews move faster when the team shares a pattern vocabulary.

## Patterns as a Shared Language for Process

One surprising insight is how process requirements map to patterns:

- **IDPM and GitHub Projects** mirror the **Kanban/Backlog** patterna visual pipeline that constrains work-in-progress. An empty M2 backlog at the deadline is the “definition of done” expressed as a workflow pattern.

- **CI with ESLint + Playwright** is the **Deployment Pipeline** pattern. Every commit triggers the same checks, reducing variability and catching regressions early.

- **Availability tests** are an application of a “test the happy path continuously” pattern. Instead of exhaustive specs, we guard the most important user journeys: load each page, submit each form.

- **Homepage badges and links** act like an **Information Radiator** pattern, making status visible without digging into logs.

Recognizing these as patterns makes me appreciate that software engineering is not just code patterns; it’s also collaboration patterns that keep the team aligned.

## Looking Forward to M3

For M3, I want us to:

- Codify the Repository layer for both reads and writes, plus role-aware query helpers.
- Adopt a consistent form pattern with a shared hook that handles validation, loading, and toastable errors.
- Enforce a clearer Container/Presentational split so UI tweaks don’t touch data logic.
- Normalize guard usage with a composable API (for example, `withAuth(Role.ADMIN)(PageComponent)`).
- Document key pattern decisions in the Developer Guide so new contributors can align quickly.

These moves will make adding new pages less painful and keep CI/Playwright stable because behavior becomes more predictable.

## What I Learned About Design Patterns Here

Coming back after M2, I’m reminded that patterns are less about academic labels and more about reducing cognitive load. They:

- Provide shared names so reviews go faster (“use the repository” beats “please move this query somewhere else”).
- Encourage consistency, which is the foundation of reliable testing and deployment pipelines.
- Make future features cheaper: when every form follows the same Template/Strategy hook, adding a new one is plug-and-play.
- Support collaboration: IDPM, CI, and availability tests are process patterns that keep the team in sync even when someone (like me during M2) is temporarily out.

Design patterns are the recipes that keep our kitchen humming. I’m excited to apply them more intentionally in M3 so that our Club Oven Lovin’ app scales in both flavor and maintainability.

## Note on AI assistance

I used an AI writing assistant to help me generate an initial draft of this essay, iterate on the structure, and polish the wording. The ideas, code references, and project-specific content are based on our Club Oven Lovin’ codebase and my own understanding, and I reviewed and edited the AI-generated draft before submitting it.
