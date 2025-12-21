---
layout: project
type: project
image: https://placehold.co/800x480?text=Club+Oven+Lovin
title: "Club Oven Lovin"
date: 2025-12-01
published: true
labels:
  - Next.js 14
  - Prisma/PostgreSQL
  - Object Storage
  - UX
  - Testing
summary: "A recipe-sharing platform with safe image uploads, resilient data flows, and a deployment-ready pipeline."
---

## Overview

Club Oven Lovin is a full-stack recipe-sharing app where students can browse, post, and revisit the dishes they love. Built on Next.js 14 with App Router, it lets users create rich recipe entries with photos, detailed steps, and tags so that cooking clubs can swap ideas without getting lost in chat threads.

The value is in the safety and polish: uploads are resized, compressed, and converted to efficient formats; data writes are validated before touching the database; and the UI guides cooks through posting without losing work. Members get a reliable place to publish, discover, and revisit recipes without worrying about storage failures or broken media.

## My Contributions

- Designed and implemented the object storage pipeline (R2/S3 compatible) with bucket policies, signed URL uploads, and lifecycle rules to avoid orphaned files.
- Added automatic WebP conversion, multi-size derivatives, and compression so recipe images load fast on both mobile and desktop.
- Built guarded form flows (server/client components) with schema validation to prevent half-written recipes and eliminate the “stuck uploading” state.
- Resolved data consistency bugs caused by Prisma schema changes, including backfills and safer migration scripts to protect existing posts.
- Hardened the review and release process: branch rules, PR templates, and test runs before merges so regressions are caught early.
- Documented issue triage and incident fixes, turning ad-hoc troubleshooting (like the accidental .env commit) into written checklists.

## What I Learned

- Requirements clarity accelerates everything: writing explicit tasks made it easier for teammates and AI agents to contribute without rework.
- Guardrails beat heroics: preventing destructive actions (schema changes without migrations, uploads without verification) is cheaper than patching data later.
- Image pipelines are systems engineering: format choice, storage cost, and UX latency are tightly linked and need measurable budgets.
- Communication cadence matters: lightweight check-ins plus issue trackers kept us from shipping surprises, even under deadline pressure.
- Process is culture: once we agreed on branch rules, code reviews, and release checklists, trust in the codebase went up and firefighting went down.

## Screenshots

![Club Oven Lovin home view placeholder](https://placehold.co/1200x720?text=Home+%2F+Browse+Recipes "Placeholder for the home page showing featured recipes")

![Recipe detail placeholder](https://placehold.co/1200x720?text=Recipe+Detail "Placeholder for a recipe detail page with ingredients and steps")

![Create recipe flow placeholder](https://placehold.co/1200x720?text=Create+Recipe+Form "Placeholder for the create recipe form with image upload")

## Links

- Organization GitHub page: [Club Oven Lovin](https://github.com/club-oven-lovin)
- Deployed site: [club-oven-lovin.github.io](https://club-oven-lovin.github.io/)
