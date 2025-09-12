---
layout: essay
type: essay
title: "When TypeScript clicked: typed thinking and timed WODs"
date: 2025-09-11
published: true
---

## Why TypeScript clicked for me
I came to TypeScript from everyday JavaScript and some Java/Python. What surprised me first was not the annotations, but **how the compiler nudges me to think**. With JS I often “just run it and see”; with TS I design the shape of data first. That shift paid off immediately in the small practice apps I wrote—like scoring exercises and a tiny stock tracker—because the *constraints* made my code simpler to trust.

Two things felt like instant wins: (1) **type inference** is generous enough that I don’t have to decorate every variable; and (2) **union types + narrowing** let me model real-world “this or that” states without booleans everywhere. Compared to plain JS, the editor feedback loop is night-and-day; compared to Java, I get types **and** the expressiveness of modern ES.

## What I learned beyond syntax (ES6 + TS together)
The module pushed me to treat ES6 features as the default: ```import/export``` for module boundaries, arrow functions for clarity, and ```const``` by default. Layering TS on top adds safer APIs. I also adopted a stricter mental model: functions are pure by default, data is immutable unless mutation buys me performance, and types describe *contracts* I can discuss with teammates.

A small example from a WOD-style scoring task was to accept user input as either numbers or strings and still return a safe result:

```ts
type JudgeScore = { judge: string; score: number | string };

export function overallScore(scores: JudgeScore[]): number | null {
  // 1) parse + validate
  const numeric = scores
    .map(s => typeof s.score === "string" ? Number(s.score) : s.score)
    .filter((v): v is number => Number.isFinite(v) && v >= 1 && v <= 10);

  // 2) need at least 3 valid scores
  if (numeric.length < 3) return null;

  // 3) drop min and max, average the rest
  const sorted = [...numeric].sort((a, b) => a - b);
  const trimmed = sorted.slice(1, -1);
  const avg = trimmed.reduce((a, b) => a + b, 0) / trimmed.length;
  return Number(avg.toFixed(1));
}
```

What I like here is how **narrowing** (the ```v is number``` predicate) lets the compiler protect later steps. In earlier JS code I would sprinkle runtime checks everywhere; with TS, the type system documents and enforces the flow.

## A quick note on classes
Re-implementing tiny OOP tasks (e.g., ```Score```, ```Ride```, simple ```Inventory```) reminded me that TS “classes” are syntax over JS prototypes, but with **real compile-time guarantees**. That means I get autocompletion, constructor contracts, and visibility modifiers without losing JavaScript’s runtime model. I still keep logic small and composable, but when a domain has identity and invariants, a class with typed fields feels appropriate.

## Athletic Software Engineering: how the WODs felt
The practice WODs were honest: **slightly stressful, very useful**. The timer exposed two habits: (1) I hesitate on boilerplate, and (2) I underestimate input validation. My fix was to keep a tiny “WOD kit” in my notes—CLI scaffolds, test snippets, and a checklist (“parse → validate → transform → format”). After two or three runs, the anxiety dropped and muscle memory formed. I also liked the instant debrief: I wrote down where I fumbled (e.g., forgetting to sort before slicing) and re-ran with a stricter time cap.

Is it enjoyable? Weirdly, yes. The bounded time box turns learning into a sport: short sprints, clear feedback, measurable improvement. I plan to keep doing *micro-WODs* (15–20 minutes) on fundamentals like array methods, generics, and error handling.

## What I’ll do next
To get the most out of TS for the rest of the course, I’m committing to:
- **```strict``` everywhere** with explicit ```unknown``` at boundaries.
- **Tests first for edge cases** (e.g., invalid inputs) so types and tests reinforce each other.
- Use **discriminated unions** for stateful code (loading/success/error) rather than boolean flags.
- Keep a personal WOD log: problem → plan → pitfall → patch → proof (tests/bench).

TypeScript made me slower for a day and faster thereafter. The WODs made that transition obvious: the more I let the type system carry the cognitive load, the more attention I have for design and naming—the parts that actually make software good.

---

*AI assistance:* I used ChatGPT to help brainstorm the outline, propose one code snippet, and proofread grammar. The reflections and final content are my own.
