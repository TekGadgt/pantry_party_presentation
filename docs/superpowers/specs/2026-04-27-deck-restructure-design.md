---
name: NorfolkJS Deck Restructure (post-feature-shipments)
description: Restructured Slidev deck for the NorfolkJS Pantry Party talk. Compresses the build recap, replaces the abstract Then→Now sidebar with three concrete post-episode feature shipments, and reframes the live demo around the AI provider toggle as the wow moment.
type: project
---

# NorfolkJS Presentation: Pantry Party — Restructure Design

**Date:** 2026-04-27
**Status:** Draft
**Speakers:** Ryan, Austin
**Target duration:** ~20 minutes (flexible within 15–25)
**Venue:** NorfolkJS meetup
**Supersedes:** `2026-04-20-norfolkjs-presentation-design.md` (kept as historical record of the pre-restructure direction)

## Why this restructure

The original deck (committed as of `80ce21a`) was scoped before the post-episode feature work landed. In the week of 2026-04-20, three feature shipments were completed on `pantry-party` (commits all dated `2026-04-20` on `upstream/main`):

1. **Auth hardening** — spec + plan + ~13 commits
2. **AI provider toggle** (OpenAI + Claude, switchable per room) — spec + plan + ~14 commits
3. **User dietary profiles** (persistent, merged into prompts) — spec + plan + ~10 commits + a standalone integration guide

The original deck has *one* slide of post-episode evidence (auth hardening) plus a 3-column "what still hurts / what works / day-to-day" thesis slide. The restructure replaces this with three concrete feature ships and lets the pattern carry the argument instead of asserting a thesis.

## Goals

- Make the **post-episode feature shipments the substance** of the talk, with the build recap compressed to a hook.
- Lean into the **meetup title** ("670K people watched us vibe code") as the opener — the audience showed up for that promise.
- Replace abstract "AI got better" framing with concrete artifacts: spec files, plan files, commits, a docs commit.
- Make the **live demo** physically demonstrate post-episode work (provider toggle flip) rather than just running the original MVP.

## Non-goals

- Word-for-word per-speaker scripts.
- Re-recording the demo backup clip. The existing `public/video/demo-backup.mp4` stub stays; live site on Netlify is reliable enough.
- Drop-in compatibility with the old slide order. Slide numbers shift; assets and components are reused where possible.
- Teaching Claude Code as a tool. The deck mentions the workflow and shows artifacts; it doesn't tutorial.

## What's changing vs. the current deck

| Section | Current | Restructured |
|---|---|---|
| Title | "Pantry Party — A 4-hour build, five months later" | "670K people watched us vibe code" |
| Opening (title → concept → Convex pivot → stack) | 6 slides (1–6) | 4 slides (1–4) — drops stack roll-call (open question), keeps title/format/concept/Convex |
| Build recap (what 4 hours looked like) | 6 slides (7–12, ~5 min): B-roll, Copilot wins, integration gap, auth-token magic-move, last 30 min, second B-roll | 2 slides (5–6, ~2 min): chaotic-energy + "We shipped. Barely." |
| Post-episode work | 5 slides (13–17, ~5 min) sidebar-framed: Then→Now, integration thesis, auth case study, "still trips," 3-column take | 7 slides (7–13, ~7 min) substance-framed: "five months later," the workflow, three-feature overview, three feature deep-dives, "what this changed" |
| Demo | 1 in-flow + 1 hidden (18, 21): audience joins → add → generate → vote | Same (14, 16), plus provider toggle flip mid-demo as climax |
| Wrap | Thanks + Q&A (20) | Same (15) |
| Total | 20 in-flow + 1 hidden | 15 in-flow + 1 hidden |

**Cuts:** "What Copilot agent mode nailed" / "Where it fell apart" framing (drops brand attribution everywhere, not just on the comparison slides), "Example: auth token didn't flow" magic-move, second B-roll slide, "Then → Now" two-column slide, "The gap that closed: integration" thesis slide, "Where it still trips" standalone slide (folds into dietary profiles), 3-column "still hurts / works / day-to-day" slide.

**Adds:** Title hook revision, "We shipped. Barely." pivot, "Five months later" transition, "The workflow" slide, "Three features in a week" overview, AI provider toggle feature slide, dietary profiles feature slide, "What this changed" single-observation slide.

**Reframed:** Existing auth-hardening slide gets a two-beat treatment — spec'd middleware → shipped client-side AuthGate (see Decision D1).

## Slide flow

~15 slides + 1 hidden backup. No hard speaker sections — handoffs are felt out live.

### Opening (~3 min)

**1. Title — "670K people watched us vibe code"**
Speaker names + NorfolkJS. CodeTV episode reference visible (view-count visual or episode screenshot). Lean into the meetup title — it's the hook the room came for. Pivot fast to substance so it doesn't read as clout-chasing.

**2. The format**
30 min plan / 4 hr build / ship something shareable. One slide, brief.

**3. What we shipped on camera**
Pantry Party in one paragraph: pool ingredients, AI recipes, vote, real-time. Screenshot or short clip. Short — the audience may already know it from the episode.

**4. The Convex pivot**
"Just use plain websockets" → "Have you tried Convex?" The architectural turn from the morning. Acknowledge in one breath that episode-watchers have seen this; keep it because it's the human moment that earns the rest.

### What 4 hours actually looks like (~2 min)

**5. The chaotic-energy slide**
B-roll or compressed war stories — env vars not propagating, Convex prod ≠ dev, Clerk JWT mismatched, recipes failing silently on prod. One beat: *"everything was on fire in the last 30 minutes."* Compresses current slides 7–11 into a single slide; the second B-roll slide (current slide 12, "Final push clip") is also dropped. **Reframing:** drops Copilot-specific attribution to keep the whole talk tooling-neutral; "AI scaffolding gets the boxes, not the wires" is the implicit subtext, not a claim about a brand.

**6. We shipped. Barely.**
Honest framing of the MVP at end-of-episode: backend auth checks commented out, a `tempUserId` hack in `addIngredient`, the Clerk JWT issuer hardcoded, four files duplicating ConvexClientProvider boilerplate, env vars manually copied around. Episode ends here. Talk doesn't.

This is the pivot slide.

### Five months later (~7 min)

**7. Five months later**
Single transition slide. "We kept building." Sets up that the rest of the talk is post-episode work the audience hasn't seen. (Title can be phrased "Five months later" or "A week ago" — both are technically true; the former matches the Nov-2025 → Apr-2026 framing the audience expects from the episode reference.)

**8. The workflow**
Spec → Plan → Commits. Show the directory structure as it actually exists on `upstream/main`:

```
docs/superpowers/specs/
  2026-04-20-auth-hardening-design.md
  2026-04-20-ai-provider-toggle-design.md
  2026-04-20-user-dietary-profiles-design.md
docs/superpowers/plans/
  2026-04-20-auth-hardening.md
  2026-04-20-ai-provider-toggle.md
  2026-04-20-user-dietary-profiles.md
```

Brief description of the loop. Don't oversell — describe.

**9. Three features in a week**
Overview slide. Three features, each with `spec + plan + N commits` callouts (commit counts approximate; finalize at draft time from `git log upstream/main`):

- **Auth hardening** — proper auth, env-driven config, deduplicated provider wrapper · spec + plan + ~13 commits
- **AI provider toggle** — Claude + OpenAI, switchable per room, provider badge on cards · spec + plan + ~14 commits
- **Dietary profiles** — persistent user preferences, merged into prompts · spec + plan + ~10 commits + 1 docs commit

Evidence the workflow scales beyond a single feature.

**10. Feature 1: Auth hardening (two-beat)**
Beat 1 — magic-move shows what was *first* spec'd: middleware with `createRouteMatcher` protecting `/room(.*)` and `/create-room`. (This is the existing slide-15 magic-move; works as-is for this beat.)

Beat 2 — punchline. The middleware approach broke modal sign-in UX (`redirectToSignIn()` is a full-page redirect to Clerk's hosted page, not the modal the app uses). Show `AuthGate.tsx` with one line of narration: *"3 of 4 hardening goals shipped clean. The fourth — route protection — swapped to a client-side gate."*

This is the workflow telling the truth about a constraint that wasn't visible at spec time. Better story than the polished version.

**11. Feature 2: AI provider toggle**
The most demo-friendly feature. One slide showing:

- Schema change: `aiProvider: v.optional(v.union(v.literal("openai"), v.literal("claude")))`
- SDK addition: `@anthropic-ai/sdk`
- UI: segmented control near the Generate button
- Provider badge on recipe cards

Meta-aside (one breath, don't dwell): *"we used Claude Code to add Claude support."*

**12. Feature 3: Dietary profiles**
New `userProfiles` Convex table, modal in navbar, locked-chip visualization (showing where each constraint came from), prompt merging.

**Embedded bug arc** — frame as in-flight texture, not a separate disclaimer slide:

> While shipping this we hit a `@clerk/astro` multi-island bug — each React island wrapping its content in `<ClerkProvider>` from `@clerk/clerk-react` spawned a duplicate Clerk instance. Real work has bugs. Switched to `useAuth` from `@clerk/astro/react`, no provider needed.

Punchline (closer): the fix shipped with `docs/clerk-astro-react-islands.md` — the next person who hits this skips the loop. Bug → fix → durable knowledge.

**13. What this changed**
Single observation slide. One framing line — examples to choose from:

- *"The shift wasn't model IQ; it was process rigor."*
- *"Prompt → diff is faster than scaffold → wire."*
- *"More time designing, less time plumbing."*

Pick one. Don't make it a thesis — make it an observation. The features have already done the arguing.

### Live demo (~6–8 min)

**14. Try it yourself**
Pre-seeded room. QR + printed URL. Live iframe of `pantryparty.lol` so the audience sees real-time updates even before signing up. Audience joins, adds ingredients, votes.

**Climax beat:** mid-demo, flip the AI provider toggle — same ingredients, both providers generate, audience sees the provider badges on the cards. This is the wow moment because it physically shows what was built post-episode.

### Wrap (~1 min)

**15. Thanks + Q&A**
URLs, repo, deck link. Keep QR up if people are still joining.

### Hidden

**16. Demo backup** (`hideInToc: true`)
Pre-recorded clip, accessible via slide overview. Existing slide retained as-is.

## Decisions

**D1 — Auth slide: tell the backed-off-the-middleware truth.**
Current `src/middleware.ts` on `upstream/main` is just `clerkMiddleware()` — no `createRouteMatcher`, no protection. The current deck's slide-15 magic-move shows code that no longer exists in the repo. The auth-hardening spec listed four goals; route protection got reverted (in `396b215`) because middleware redirect breaks modal sign-in UX. Three of four shipped clean. Two-beat magic-move tells this truthfully: spec'd middleware (before/after) → shipped AuthGate. Same screen real estate, more honest, better workflow story.

**D2 — Bug slide: combined "real work has bugs" framing + docs-commit punchline.**
Lead with the calibration ("bug surfaced mid-feature, normal"), close with the capability claim ("...and the fix shipped with a docs commit, so the next person skips the loop"). The docs commit is `581b1c7 docs: add Clerk + Astro + React islands integration guide` — it's a real, substantive artifact (~100 lines, before/after code, key rules). Calibration without the punchline leaves evidence on the table. Punchline without calibration risks overselling.

**D3 — Demo backup: keep the existing hidden backup slide; do not re-record.**
Live site on Netlify is reliable; user is confident. Existing `public/video/demo-backup.mp4` stub + slide-21 hidden backup remain. **Risk acknowledged:** the provider-toggle climax doubles the live failure surface (both OpenAI and Anthropic must be healthy at showtime). Mitigation: pre-talk-day API smoke test (see Rehearsal plan).

**D4 — Drop Copilot-vs-Claude-Code brand comparison everywhere.**
Removing the comparison from slide-15 only would smuggle the framing back in via slides 8–9 ("What Copilot agent mode nailed" / "Where it fell apart"). The whole talk goes brand-neutral on the build-recap side. Subtext is "AI scaffolding gets boxes, not wires," not "Copilot specifically failed at X."

**D5 — Keep one explicit "what changed" framing line (Codex's note).**
Without it, the audience has to infer causation across three feature slides — too much to ask in 20 minutes. Slide 13 carries this load with one observation line, no thesis.

**D6 — "Three features in a week" timeline phrasing is accurate.**
All post-episode commits dated `2026-04-20` on `upstream/main`. Today is `2026-04-27`. "In a week" understates the concentration if anything.

**D7 — Title: "670K people watched us vibe code".**
The meetup title sets the audience expectation; the deck should match. Lean into the view count + episode reference. Pivot to substance fast (build recap is now 4 slides, not 6) so the title pays off rather than overshadowing.

## Audience-demo design

Same as the original spec's design:

- QR code + printed URL → `https://pantryparty.lol/room/<code>` (direct room URL; Clerk's modal triggers and returns user post-signup).
- Pre-seeded room with 5–8 ingredients and reasonable dietary defaults.
- Live iframe on the demo slide so the audience sees real-time updates pre-signup.

**New climax:** mid-demo, flip the AI provider toggle. Trigger generation on both providers (sequentially or with the same ingredient set in another room). Audience sees provider badges on the resulting recipe cards. This is the moment that makes the post-episode work tangible.

**Backup path:** unchanged — slide-21 hidden backup clip if live site misbehaves.

## Failure modes & mitigations

| Risk | Mitigation |
|---|---|
| Either OpenAI or Anthropic API down/rate-limited at showtime | Smoke-test both providers ~1h before talk; pre-generate one set per provider as fallback (audience still sees provider badges, just not live) |
| Provider toggle UI doesn't reflect change in time | Already addressed in shipped feature: `0fddc30 fix: show Generating... text for all users via room status` |
| Live site is down | Existing slide-21 backup clip |
| Wi-Fi is bad for audience | Drop audience-join; presenters drive from two stage devices |
| Title hook reads as clout-chasing | Pivot to substance within the first ~20 seconds; build recap is now 4 slides specifically to make this work |
| Audience thinks the workflow story is a Claude Code ad | Slide 13's framing line is "process rigor," not tool branding (D5) |
| @clerk/astro bug recurs during live demo | Already fixed and documented (commits `73f90b2`, `581b1c7`); Convex schema is on `upstream/main` and Netlify deploys from there |

## Rehearsal plan

- Full end-to-end run from both laptops, time each section. Cut content if over by >3 min.
- **Provider-toggle smoke test ~1h before talk:** generate one set with each provider in the demo room; confirm both succeed and the badge renders.
- Audience-demo concurrency dry-run: 10–15 simultaneous joins via incognito/secondary devices the week before.
- Run through the deck out loud at least once each before doing it together — handoffs are felt out live, but each speaker should know all the slides cold.

## Asset checklist (delta from original spec)

| Asset | Status |
|---|---|
| Episode screenshot or view-count visual for slide 1 | **NEW** — needed for title slide |
| Workflow directory-structure visual for slide 8 | **NEW** — can be a code block, screenshot of file tree, or simple list |
| Provider badge screenshot for slide 11 | **NEW** — pull from the live app |
| Locked-chip visualization screenshot for slide 12 | **NEW** — pull from the live app |
| `docs/clerk-astro-react-islands.md` thumbnail or quoted line for slide 12 | **NEW** — file exists on `upstream/main` |
| CodeTV B-roll clip for slide 5 | Pending (carried from original spec) |
| BTS photos | Ready (carried) |
| Live site | Live (carried) |

## Open questions

1. **Stack roll-call slide.** Current deck has a slide-6 stack grid (Astro / React / Convex / Clerk / OpenAI / Tailwind). Restructured deck cuts it for tightness; stack components surface organically through the workflow + feature slides anyway. Keep, cut, or absorb into another slide? Default in this spec: cut.
2. **Title slide visual.** View-count number ("670K") prominent, or episode screenshot prominent, or both? Does Austin want input on the visual treatment?
3. **"What this changed" line — final wording.** Three options listed in slide 13. Pick one before drafting.
4. **"Five months later" vs "A week ago"** for slide 7. Both technically true. Five-months framing matches the audience's mental model from the episode reference; "a week ago" is more arresting. Check with Austin.

## Out of scope

- Re-recording the demo backup clip (D3).
- `pantry-party` code changes for the talk (e.g., demo-mode rate limiting, anonymous join). Tracked separately on that repo.
- Custom Vue components beyond `qrcode.vue` (already in deps).
- Migrating to a non-`seriph` theme. Easy swap if a speaker dislikes it during rehearsal; not a structural decision.

## Implementation note

The original spec's Architecture, Package scripts, and Speaker notes sections still apply unchanged — see `docs/superpowers/specs/2026-04-20-norfolkjs-presentation-design.md` for that material. This document covers only what's changing.
