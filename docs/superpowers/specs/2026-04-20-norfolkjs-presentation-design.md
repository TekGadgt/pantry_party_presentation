# NorfolkJS Presentation: Pantry Party — Design

**Date:** 2026-04-20
**Status:** Draft
**Speakers:** Ryan, Austin
**Target duration:** ~20 minutes (flexible within 15–25)
**Venue:** NorfolkJS meetup

## Goal

Tell the story of Pantry Party — a real-time collaborative recipe app built in 4 hours at the CodeTV full-stack challenge — with an AI-tooling-then-vs-now sidebar and a live multiplayer audience demo as the climax.

Thesis: **"We built a thing, here's the journey."** The AI-tooling shift (Copilot agent mode → Claude Code) is a sidebar, not the climax.

## Non-goals

- Live bug-fix demo. The auth hardening originally pitched as the live Claude Code demo has already shipped on the `pantry-party` repo (commits `3abd7c5`, `5af3a3c`, `e6ac57f` on 2026-04-20). We reference those artifacts as case-study evidence instead.
- Deep Claude Code tutorial. The talk mentions the tool shift and shows evidence; it doesn't teach the tool.
- Rigid per-section speaker assignments. Handoffs are fluid.

## Architecture

Slidev deck scaffolded in `/home/tekgadgt/work/pantry_party_presentation/`, its own git repo (separate from `pantry-party`).

### Directory structure

```
pantry_party_presentation/
├── slides.md              # the deck itself; slide breaks via ---
├── components/            # optional Vue components for custom interactives
├── public/
│   ├── video/             # CodeTV clips
│   ├── photos/            # BTS photos
│   └── screenshots/       # tooling-sidebar captures
├── snippets/              # code snippets pulled into slides via <<< syntax
├── package.json
├── netlify.toml           # deploy config (added once draft is ready)
├── docs/superpowers/
│   ├── specs/             # this file
│   └── plans/             # implementation plan (produced next)
└── README.md              # run/build/export instructions for both speakers
```

### Framework & tooling

- **Slidev** — Markdown-based deck framework. Trivial iframe embeds (critical for the audience-demo slide), Shiki code highlighting with magic-move, presenter mode with notes, hot reload via Vite, PDF/PNG export, static SPA build.
- **Theme:** `@slidev/theme-seriph` (start here; one-line swap if it doesn't feel right).
- **Node:** 18+.
- **Deploy:** Netlify, mirroring the `pantry-party` stack.
- **Version control:** git, both speakers clone and edit `slides.md` directly.

### Package scripts

| Script | Command | Purpose |
|---|---|---|
| `dev` | `slidev` | Local presenter, hot reload |
| `build` | `slidev build --base /` | Static SPA in `dist/` |
| `export` | `slidev export` | PDF of full deck (offline backup) |
| `export-notes` | `slidev export-notes` | PDF of speaker notes |

## Slide flow

~20 slides, ~20 minutes (4 opening + 5 build + 4 sidebar + 6–8 demo + 1 wrap). Soft speaker leans noted; handoffs are fluid.

### Opening / The Setup (~4 min, Ryan + Austin)

1. **Title** — "Pantry Party" + speakers + NorfolkJS
2. **The hook** — one-liner on the 4-hour CodeTV challenge; BTS photo backdrop
3. **The format** — CodeTV's rules: 30 min to plan, 4 hours to build, ship something shareable
4. **The concept** — what Pantry Party is: pool ingredients, AI recipes, vote. Default to a BTS photo or UI shot; upgrade to a video clip if one is extracted in time.
5. **The Convex pivot** — "we walked in planning plain websockets"; Jason suggested Convex; we adopted a real-time DB neither of us had touched
6. **Tech stack roll call** — logo grid: Astro, React, Convex, Clerk, OpenAI, Tailwind

### The Build (~5 min, Austin leads)

Trimmed from original 7 min to give the demo room to breathe. Stay crisp — one beat per slide.

7. **CodeTV B-roll clip** — setting the scene
8. **What Copilot agent mode did well** — scaffolding individual pieces: Astro page, Convex schema, Clerk setup, OpenAI call
9. **The integration gap** — diagram: four boxes (Clerk / Convex / OpenAI / UI) with wiring between them highlighted in red; this is where things broke
10. **Specific breakages** — 1–2 concrete examples with code snippets; Shiki with line highlights
11. **Late-stage scramble** — deployment pain, env vars, demo-readiness
12. **CodeTV clip** — the final push moment

### Then vs. Now — Tooling Sidebar (~4 min, Ryan leads)

13. **Transition slide** — "Then → Now" (big visual beat)
14. **The landscape shift** — Copilot agent mode (best tool in Nov 2025, Anthropic + OpenAI models under the hood) → Claude Code (daily driver now)
15. **Where it got better** — the integration problem is exactly where tooling improved most
16. **Case-study evidence** — auth hardening: spec → plan → commit chain screenshots from `pantry-party` repo. Slidev magic-move for middleware before/after.
17. **Honest take** — what AI still struggles with, what's surprisingly good, what changed day-to-day

### The Demo (~6–8 min, either driver)

18. **Try it yourself** — big QR code linking to `https://pantryparty.lol/room/<code>` (the direct room URL; Clerk's auth modal triggers on the way in and returns the user to the room post-signup). Same URL printed in large text beneath the QR as a scan fallback. Live iframe embed of the site below that. Audience signs up with username/password (fast path; ~15–20s per user, parallelized). Slide stays up while the presenter drives the audience through: adding ingredients → triggering recipe generation → voting live. The connected-users display updates in real time.
19. **Backup clip (hidden)** — 30-second pre-recorded end-to-end demo, revealable on demand if live site misbehaves. Not shown in normal flow.

### Wrap (~1 min)

20. **Credits + links** — repo URLs, deck URL, speaker contact; Q&A prompt

## Audience-demo design

**Primary path:** Audience joins for real. QR code (and matching printed URL) on slide #18 point directly to `https://pantryparty.lol/room/<code>`. Unauthenticated users hit Clerk's auth modal, complete u/p signup (~15–20s), and are returned to the room. The iframe on slide #18 shows the live site so the audience can see real-time updates even before they've joined.

**Auth friction:** Sign-up is u/p (no OAuth gate, no email verification needed for demo purposes). Expect ~15–20 seconds per user, parallelized across the room.

**Pre-seed:** Presenter creates the demo room before going on stage and pre-seeds it with a handful of ingredients (so the recipe-generation payoff is fast even if audience contribution is light). Dietary constraints set reasonable defaults. Room code/URL hardcoded into slide #18.

**Backup path:** If live site misbehaves, a 30-second pre-recorded demo clip lives on slide #19 — revealable on demand via Slidev navigation without breaking the normal click flow.

## Failure modes & mitigations

| Risk | Mitigation |
|---|---|
| Live site is down | 30s pre-recorded demo clip on slide #19 as fallback |
| Wi-Fi is bad for audience | Drop audience-join; presenters drive from two stage devices, demo is still multi-user |
| OpenAI is slow / rate-limited | Trigger recipe generation before the demo slide, or have a pre-generated room ready |
| Video clips don't play | Static fallback image (representative frame) behind each `<video>` tag |
| Presenter laptop drops | Deck on shareable Netlify URL; other presenter drives from their laptop |
| QR code doesn't scan | Full room URL (`pantryparty.lol/room/<code>`) printed beneath the QR — same destination as the QR |
| Clerk sign-up fails for a user | Not a talk-blocker; presenters keep driving; mention as a known rough edge |

## Speaker notes

Slidev supports per-slide notes in `<!-- ... -->` blocks at slide foot; render only in presenter view. Each slide gets 1–3 lines as soft cues (not scripts), matching the "no hard cutoffs" preference.

## Rehearsal plan

- Full run-through end-to-end from both laptops
- Time each section; cut content if over by >3 min
- Dry-run the audience demo with 2–3 live people joining from phones to sanity-check the flow
- Concurrency dry-run: emulate 10–15 simultaneous joins using multiple browsers / incognito windows / secondary devices between the two speakers. Exercises Clerk signup burst, Convex subscription scale, and UI behavior under load that 2–3 phones won't surface. Run at least once the week before the talk.

## Assets checklist

| Asset | Source | Status |
|---|---|---|
| CodeTV video clips | Speakers will extract from episode | Pending (speaker task) |
| BTS photos | Speakers have them | Ready |
| Copilot agent mode screenshots | Not captured at event; skip unless extractable from video | Low priority |
| Live site | `https://pantryparty.lol/` | Live |
| Auth-hardening artifacts | `pantry-party` repo: `docs/superpowers/specs/2026-04-20-auth-hardening-design.md`, `docs/superpowers/plans/2026-04-20-auth-hardening.md`, commits `3abd7c5`, `5af3a3c`, `e6ac57f` | Ready |

## Out of scope

- `pantry-party` code changes (e.g., anonymous/guest join mode, demo-specific rate limiting). Those are separate work tracked on that repo.
- Per-speaker word-for-word scripts.
- Custom Vue components. Start with Slidev defaults; add components only if a slide demonstrably needs one.
- Slide animations beyond what Slidev provides out of the box.

## Open questions

None blocking. Theme (`seriph` vs alternatives) is an easy swap post-scaffold.
