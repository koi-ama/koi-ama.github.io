---
layout: essay
type: essay
title: "Reflecting on AI in ICS 314: WODs, Club Oven Lovin’, and Beyond"
date: 2025-12-15
published: true
tags: [ai, reflection, ics314, learning]
---

## I. Introduction

AI showed up everywhere in ICS 314 this semester—from Next.js WODs to the Club Oven Lovin’ final project. I leaned on ChatGPT-style agents to plan tasks, generate prompts for teammates, and sanity-check Prisma changes. Tools like GitHub Copilot and browser-based chatbots helped, but I still kept the human-in-the-loop mindset: draft with AI, verify with docs and tests.

## II. Personal Experience with AI

1. **Experience WODs e.g. E18**  
   For the Experience WOD E48B (`nextjs-example-form`), I pasted my server action and asked (Japanese, then translated), *“dbActions現状これ：『…』これをどうすればいいの。これにすればいいの？”* / “Here’s my `dbActions` upsert; should I change it to the version with `instructor`?” The agent returned a Prisma `upsertStudent` like:
   ```ts
   export const upsertStudent = async (data: ICreateStudentForm) => prisma.studentData.upsert({
     where: { email: data.email },
     update: { name: data.name, bio: data.bio ?? null, level: data.level, major: data.major, gpa: data.gpa, hobbies: data.hobbies, instructor: data.instructor },
     create: { name: data.name, email: data.email, bio: data.bio ?? null, level: data.level, major: data.major, gpa: data.gpa, hobbies: data.hobbies, instructor: data.instructor },
   });
   ```
   Benefit: quick Prisma syntax confirmation for enum casts and optional fields. Cost: I still re-checked schema enums and validation to avoid mismatches.

2. **In-class Practice WODs**  
   During `nextjs-dollar`, I prompted, *“実装担当aiに渡すためのコマンド作成してほしい。添付している3枚の画像はwebの完成後の状態で、実装担当aiにも渡す予定。”* / “Create shell commands I can hand to another AI to finish the WOD; screenshots show the final UI.” The reply included exact commands:
   - `dropdb nextjs-dollar --if-exists && createdb nextjs-dollar`
   - `npx prisma migrate dev --name add-value-to-stuff`
   - `npx prisma db seed`
   Plus a JSX hint for the new money field:  
   ```tsx
   <Form.Control type="number" name="value" step="0.01" required />
   ```
   Benefit: faster setup and consistent UI fields. Cost: I adjusted migration names to match the repo history.

3. **In-class WODs**  
   I mostly skipped AI here to practice raw speed; switching windows cost time. When tempted to ask for syntax help, I instead relied on memory and starter code to stay within the timer.

4. **Essays**  
   I used AI to draft outlines, with a prompt like *(approximate)* “Give me a concise outline for an ICS 314 reflection essay using headings I–VIII.” Benefit: structure in seconds. Cost: extra passes to add my own voice and course-specific evidence.

5. **Final project**  
   For Club Oven Lovin’, I asked *(approximate)* “Propose a deployment plan that keeps our recipe DB safe while we iterate on UI consistency—include staging and rollback steps.” The agent suggested a two-environment flow (staging → prod) and nightly dumps. Benefit: surfaced a staged backup plan and checklist (“`pg_dump` nightly, rotate S3 bucket, add health checks”). Cost: it assumed AWS services we didn’t use; I pruned it to our simpler hosting and added a manual restore script.

6. **Learning a concept / tutorial**  
   When server actions felt opaque, I requested *(approximate)* “Explain how Next.js server actions keep Prisma connections short-lived.” Benefit: clarified lifecycle. Cost: generic examples meant I still read the official docs.

7. **Answering a question in class or in Discord**  
   I avoided AI mid-discussion to keep answers grounded in the shared starter repos. Instead, I skimmed the class materials to respond quickly without risking hallucinated guidance.

8. **Asking or answering a smart-question**  
   Before posting, I had AI review a draft *(approximate)* “Is this a smart question about Prisma decimal vs float tradeoffs for currency fields?” Benefit: tightened wording. Cost: minor time overhead compared to posting directly.

9. **Coding example e.g. “give an example of using Underscore .pluck”**  
   I didn’t use AI for standalone examples because the WOD instructions already provided patterns; copying them kept me aligned with grading expectations.

10. **Explaining code**  
    I fed the `upsertStudent` snippet to AI to narrate control flow and confirm both tables were updated. Benefit: rapid validation. Cost: I still traced the generated Prisma calls against the schema to be sure.

11. **Writing code**  
    AI drafted initial server-action stubs for `nextjs-example-form` using the prompt above. Benefit: reduced typing. Cost: I rewrote parts to match my validation schema and ESLint rules.

12. **Documenting code**  
    Minimal use: I preferred writing comments myself to keep them context-aware. AI-produced comments felt too generic, so I skipped it after one trial.

13. **Quality assurance e.g. “What’s wrong with this code <code here>” or “Fix the ESLint errors in <code here>”**  
    For `nextjs-dollar`, I asked *(approximate)* “Why is ESLint complaining about missing dependencies in `useEffect`?” Benefit: quick reminder to include deps. Cost: the advice was broad; I still inspected the hook manually.

14. **Other uses in ICS 314 not listed**  
    Timeboxing: I used AI to generate 20–25 minute checklists for WOD prep *(approximate prompt)* “Give me a pre-flight list before running `npm run lint` and `npm run dev`.” Benefit: reduced forgotten steps. Cost: small setup time.

## III. Impact on Learning and Understanding

Thinking ahead about architecture and task order became my default. Before coding, I now sketch the data flow and then hand that plan to an AI agent; the clearer prompt yields clearer code suggestions. Breaking work into smaller tasks (like “add Prisma field” → “update form” → “update list view”) improved accuracy and made agent feedback easier to verify. Overall it was “good things all around,” but only because I double-check outputs against docs and tests.

## IV. Practical Applications

Outside ICS 314, Quizlet’s paywall on study sessions nudged me to build an Anki-like app. I used AI to go from spec to requirements, then to an implementation plan, coding, and verification. It also helped sketch infrastructure (auth + storage + sync). The multi-service integration still needed manual “bridging” between APIs—AI could propose steps, but I wired tokens and callbacks myself. The result: my data stayed free, and the agent sped up decisions without owning them.

## V. Challenges and Opportunities

Using AI inside the dev workflow was easy and the benefits were obvious (faster drafts, fewer typos). Embedding AI into the product itself felt harder—gaps, wrong assumptions, and lack of environment awareness showed up when services had to interact. That friction signals a need for stronger dev agents and better tool integration.

## VI. Comparative Analysis

With Python, I rarely read the C implementation beneath the interpreter. Future AI use might look similar: trust higher-level outputs and skim logic/implementation reports instead of every line. Today that still feels risky, so I verify more thoroughly—lint, tests, and schema diffs—before accepting suggestions.

## VII. Future Considerations

Software engineering education may drift upward: less emphasis on low-level mechanics, more on architecture, evaluation, and directing agents. If AI handles syntax, instructors can focus on design tradeoffs and verification strategies students must still own.

## VIII. Conclusion

AI helped me complete WODs, polish Club Oven Lovin’, and start a personal Anki replacement. The gains came from planning first, prompting clearly, and verifying everything. That mix kept me fast without outsourcing responsibility.
