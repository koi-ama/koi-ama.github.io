---
layout: project
type: project
image: ../img/updatingMobile.gif
title: "yamazemi.info — the site that got me into the seminar, and why I’m redesigning it"
date: 2025
published: true
labels:
  - Web
  - Design
  - Community
  - Seminar
  - Keio
summary: "I built yamazemi.info because it was the website that nudged me into the research seminar. It’s since become our little bulletin board for announcements and notes—and I’m shipping a mobile-first redesign (see the two GIFs)."
---

## TL;DR

- **yamazemi.info** started as a simple, public home for the seminar’s info.  
- It’s also *my origin story*: reading it convinced me to join the group.  
- Today it’s a steady place for announcements, talk notes, and links.  
- I’m rolling out a **mobile-first redesign** to make it easier to skim on phones.

<!-- Page-scoped styles (you can move to site CSS later) -->
<style>
  .link-card {
    position: relative;
    display: flex;
    gap: 16px;
    align-items: center;
    border: 1px solid #e5e7eb;
    border-radius: 12px;
    padding: 12px;
    background: #fff;
    text-decoration: none;
    transition: transform .06s ease, box-shadow .2s ease, border-color .2s ease;
    max-width: 820px; /* お好みで */
    margin: 12px 0;
  }
  .link-card:hover { transform: translateY(-1px); box-shadow: 0 6px 24px rgba(0,0,0,.08); border-color:#d1d5db; }
  .link-card-thumb {
    flex: 0 0 96px; width:96px; aspect-ratio:1 / 1;
    border-radius: 10px; overflow: hidden; background:#f3f4f6;
    display:grid; place-items:center;
  }
  .link-card-thumb img { width:100%; height:100%; object-fit:cover; display:block; }
  .link-card-body { min-width:0; }
  .link-card-title { font-weight:700; font-size:1.05rem; margin:0 0 4px; color:#111827; }
  .link-card-desc  { margin:0; color:#6b7280; font-size:.95rem; }
  .link-card-url   { margin-top:6px; color:#4f46e5; font-weight:600; font-size:.95rem; }
  /* 全面クリック用の覆いリンク */
  .link-card > .overlay {
    position:absolute; inset:0; border-radius:12px; z-index:5;
  }
  /* テキスト選択やフォーカスのため本文は上に */
  .link-card-body, .link-card-thumb { position:relative; z-index:2; }

  @media (max-width:640px){
    .link-card { flex-direction:column; align-items:stretch; }
    .link-card-thumb{ width:100%; aspect-ratio:16/9; }
  }
  @media (prefers-color-scheme: dark) {
    .link-card { background:#0b0b0c; border-color:#26272b; }
    .link-card-title { color:#e5e7eb; }
    .link-card-desc { color:#9aa0a6; }
  }
</style>

<!-- Link Card (uses /img/yamazemi_logo_celeste_square.png) -->
<div class="link-card" role="group" aria-label="yamazemi.info">
  <div class="link-card-thumb">
    <img
      src="{{ '/img/yamazemi_logo_celeste_square.png' | relative_url }}"
      alt="yamazemi.info logo" loading="lazy" decoding="async">
  </div>
  <div class="link-card-body">
    <p class="link-card-title">yamazemi.info</p>
    <p class="link-card-desc">Seminar site — announcements, notes, and how to join.</p>
    <div class="link-card-url">yamazemi.info ↗</div>
  </div>
  <a class="overlay" href="https://yamazemi.info" target="_blank" rel="noopener" aria-label="Open yamazemi.info"></a>
</div>



---

## Why I made it

I first found the seminar through a plain, unassuming page—meeting times, recent talks, and a few write-ups. That page lowered the barrier to reach out. No mystery, no gatekeeping; just “here’s what we do.”  
I built **yamazemi.info** to keep that feeling alive: a single, tidy place where a curious student can land, catch the vibe, and think, *“I could be here.”* That’s exactly what happened to me, and I want to keep that door open for the next person.

## What lives on the site

- Upcoming sessions, quick summaries, and slides/links  
- Recaps and reading lists from members  
- “How to join” info with expectations and contact details  
- A lightweight archive so newcomers can see the breadth over time

It’s intentionally simple (plain HTML/CSS with a pinch of JS). Editorial friction is near zero, so people actually post.

---

## Ongoing redesign (mobile-first)

The old layout worked on desktop but felt cramped on phones. I’m refactoring the typography scale, spacing, and navigation to match how people actually read: 30–60 seconds between classes, on a small screen.

<div style="max-width:80vw; margin:0 auto;">
  <div style="display:flex; flex-wrap:wrap; gap:16px; justify-content:center; align-items:flex-start;">
    <figure style="flex:1 1 360px; margin:0;">
      <img src="{{ '/img/updatingMobile.gif' | relative_url }}" alt="Mobile layout update demo" style="width:100%; height:auto; display:block; border-radius:8px;">
      <figcaption style="margin-top:6px; text-align:center;">Mobile layout refresh</figcaption>
    </figure>
    <figure style="flex:1 1 360px; margin:0;">
      <img src="{{ '/img/updatingMobileBurger.gif' | relative_url }}" alt="Mobile burger menu interaction demo" style="width:100%; height:auto; display:block; border-radius:8px;">
      <figcaption style="margin-top:6px; text-align:center;">Burger menu interaction</figcaption>
    </figure>
  </div>
</div>

### Design goals

- **Readable on the go:** larger line-height, comfortable margins, better contrast  
- **Zero-hunt navigation:** a compact header with a clear “Join / Contact” path  
- **Scannable posts:** consistent titles, metadata, and “one-screen” summaries  
- **Lightweight:** no heavy frameworks; fast on campus Wi-Fi (and off it)

---

## Why it matters

A seminar is more than weekly meetings; it’s the conversations before and after. The site is where announcements, drafts, and invitations live in public. It makes the group visible to future members and lowers the social cost of saying “hi.” For me, that visibility was the difference between *thinking about it* and *joining*.

---

## What’s next

- Better archives (filter by topic/semester)  
- A “Start here” page for prospective members  
- Small accessibility passes (skip links, focus states, prefers-reduced-motion)
