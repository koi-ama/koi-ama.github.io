---
layout: essay
type: essay
title: "Final Project Idea"
date: 2025-11-04
published: true
labels:
  - Software Engineering
  - Nextjs
---

# MānoaLingo

## Authors
- [Koichiro Amakasu](https://koi-ama.github.io/)
- [Miron Suarez](https://mironsuarez.github.io/)
- [Chloe Teijeiro](https://chloe-teijeiro.github.io/)
- [Nathan Vogel](https://ndv0gel.github.io)
- [Kevin Lee](https://leek7.github.io/)

## Overview
*The problem:* Many UHM students meet new foreign-language words in classes, conversations, or media, but it’s hard to capture them quickly, keep the context, and review consistently.

*The solution:* **MānoaLingo** lets students instantly save words/phrases (text or voice). In the background, AI enriches each entry with **definitions**, **translations**, and **multiple usage examples** written for different tones/situations (e.g., **casual**, **polite/formal**, **academic**), plus **tags** (food, campus life, travel) and **timestamps**. The app then provides gentle flashcard-style reviews so students can study by **topic** or by **month**.  

*Inspiration:* the quick-capture feel of **[nani.now](https://nani.now)** (by Catnose) and the **game-like periodic review** loop popularized by **[Quizlet](https://quizlet.com)** (and similar apps), adapted for vocabulary learning in busy campus life at UHM.

## Approach
Once a word/phrase is created, other pages let users browse and study:
- **Landing page** — What the app is and why it helps.
- **Capture page** — Add via text or voice; show AI-enriched fields including tone-specific examples (casual/formal/etc.).
- **Library page** — Browse saved items; filter by tag/date.
- **Study mode** — Flashcard-style review with a light spaced-repetition cadence and progress bars.
- **Settings page** — Review frequency, language preferences, export, and **contact email** for reminders.

> As this is a **web app** (not a native app), we’ll use **email reminders** as a notification stand-in to prompt periodic study until in-browser notifications are enabled.

Admins (in later iterations) can monitor inappropriate content and curate tag categories.

## Use case ideas
- A student hears a Japanese phrase on campus, adds it with voice; MānoaLingo stores it with definition, **casual** and **formal** examples, and a **campus life** tag.
- Before a trip, the student filters **travel** and runs a quick study session.
- At month-end, the student reviews everything captured in **October** after receiving an email reminder.

## Progress tracking
- Track completion by **collection** (per tag or month): e.g., *“Campus Life - Cycle 1: 100% (each item answered correctly twice); Cycle 2: 80% complete.”*
- Per-item history (again/hard/good), next-review date, streaks, and gentle gamified cues.

## Beyond the basics
After the core flow works, possible enhancements include:
- Pronunciation playback or speech-practice.
- “Word streaks” and gentle reminders.
- Export study lists (CSV/Anki).
- Community word boards to discover trending vocabulary.

---
