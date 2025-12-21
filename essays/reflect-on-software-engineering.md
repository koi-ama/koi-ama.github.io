---
layout: essay
type: essay
title: "Reflect on Software Engineering: Shipping Club Oven Lovin"
date: 2025-12-02
published: true
---

## Introduction

Building Club Oven Lovin forced me to treat software engineering less like a class assignment and more like a public product. The project combined appetizing UI, strict data protections, and a delivery deadline, so every decision—from image uploads to release cadence—had to respect real users instead of checkboxes.

## Process: From Requirements to Delivery

We started by turning the vague goal of “a recipe hub” into explicit user stories and acceptance criteria. That discipline let us map features to Next.js App Router pages, schema changes in Prisma, and object storage needs for photos. Implementation was incremental: scaffold the pages, wire Prisma migrations, add guarded upload endpoints, then layer on responsive UI polish. Each merge required a review and a short release note so deployment on Vercel stayed predictable.

## Collaboration & Project Management

Tasks lived in issues with owners, estimates, and definition of done. Branches followed the same language, making reviews easier and enabling AI agents to help with checklists and summaries. We learned the cost of skipping protocols when someone committed `.env`; after that, we enforced protected branches, PR templates, and a routine of posting blockers in chat so nothing silently stalled.

## Quality & Risk Management

The riskiest parts were schema changes and file uploads. We instituted migrations with backups, required seed data to be idempotent, and added guards so file uploads expired if validation failed. Links and media were checked before merges to avoid the broken-image rot that plagues recipe sites. Automated linting plus manual smoke tests on preview deployments caught regressions before they reached users.

## Trade-offs & What I’d Do Next

We chose WebP + multiple sizes to balance quality and cost, accepting a slightly longer upload step to guarantee fast reads. Sticking with Prisma over raw SQL sped up iteration but limited some optimizations; next, I’d add query-level observability and edge caching for popular recipes. I’d also automate content moderation and add a guided onboarding so first-time posters don’t bounce.

## Conclusion

The project reinforced that software quality is mostly about habits: clear tasks, guarded workflows, documented recoveries, and honest communication. Those practices would apply just as well to an e-commerce launch or an internal dashboard—anywhere real people rely on the software to behave.
