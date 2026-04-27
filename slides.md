---
theme: the-unnamed
title: Pantry Party — NorfolkJS
info: 670K people watched us vibe code (and what we did about it)
class: text-center
background: /photos/qYx8NX49OS8-HD.jpg
---

# 670K people watched us vibe code

<div class="pt-6 text-2xl opacity-90">
  Pantry Party · 4 hours · CodeTV
</div>

<div class="pt-12 opacity-75 text-xl">
  Ryan · Austin · NorfolkJS
</div>

<!--
Hook. Reference the meetup title and the episode. 

Pivot fast — build recap is short, the post-episode work is the substance. 

If a CodeTV thumbnail screenshot becomes available, swap to a `layout: image` with the screenshot as backdrop.
-->

---
layout: about-me

helloMsg: Hey Y'all!
name: Austin Galyon
imageSrc: /photos/ag-webdev-cropped.jpeg
position: left
job: Software Engineer
line1: 
line2: 
social1: 
social2: 
social3: 
---

<!--
Hi I'm Austin, etc
-->

---
layout: about-me

helloMsg: Howdy!
name: Ryan McGovern
imageSrc: /photos/IMG_3831.jpg
position: right
job: Software Engineer
line1: "The AI Collective"
line2: "cssdaily.dev"
social1: "@tekgadgt(.dev) ryanmcgovern.dev"
social2: "cal.com/tekgadgt ryan@tekgadgt.com"
---

<!--
Hi I'm Ryan, etc
-->

---
layout: center
---

# CodeTV's rules

- **30 minutes** to plan
- **4 hours** to build
- Ship something **shareable**

<div class="pt-8 text-sm opacity-60">Full-stack challenge episode</div>

<!--
Quick primer on the CodeTV format. Don't dwell — audience gets the shape.
-->

---
layout: two-cols
---

# What we shipped on camera

Pantry Party — collaborative rooms where people:

- 🥗 Pool pantry ingredients
- 🤖 Generate AI recipes
- 🗳️ Vote on favorites
- ⚡ See updates in real time

::right::

<img src="/photos/placeholder-hero.svg" class="rounded-lg shadow-xl" />

<!--
Past tense — this is what shipped on the episode. Audience may already know it from the show. If a video clip is ready, swap image src to /video/<file>.mp4 with a <video> tag.
-->

---
layout: quote
---

# "Just use plain websockets."

_— us, that morning_

<v-click>

## "Have you tried Convex?"

_— Jason, 15 minutes in_

</v-click>

<v-click>

We adopted a real-time database neither of us had touched.

</v-click>

<!--
Biggest moment of hour one. The whole architecture swung on this suggestion.
-->

---
layout: center
class: bg-black text-white
---

# What 4 hours actually looks like

<div class="pt-8 text-2xl">
  <div>First 3 hours: scaffolding worked.</div>
  <v-click>
    <div class="pt-6">Last 30 minutes: everything was on fire.</div>
  </v-click>
</div>

<v-click>

<div class="pt-8 text-base opacity-70 max-w-2xl mx-auto text-left">
  Env vars not propagating · Convex prod ≠ dev · Clerk JWT mismatch · recipes failing silently · a lot of manual copying
</div>

</v-click>

<!--
Compress the build chaos into two beats. AI scaffolding got the boxes; integration is where 4-hour builds bleed. If a CodeTV B-roll clip becomes available, swap the layout for a `<video>` tag and let the chaos play under the text.
-->

---
layout: center
---

# We shipped. Barely.

<div class="pt-6 text-lg opacity-80 max-w-2xl mx-auto text-left">
At the end of 4 hours we had:
</div>

<div class="pt-4 text-lg max-w-2xl mx-auto text-left">

- Backend auth checks commented out (`tempUserId` hack in `addIngredient`)
- Clerk JWT issuer hardcoded
- `ConvexClientProvider` duplicated across 4 files
- Env vars manually copied around

</div>

<v-click>

<div class="pt-10 text-2xl text-center">The episode ended here. The talk doesn't.</div>

</v-click>

<!--
Honest framing. The MVP shipped, but it shipped rough. This is the pivot — episode ends here, talk doesn't.
-->

---
layout: center
class: text-center
---

# Five months later

<div class="text-xl opacity-60 mt-8">We kept building.</div>

<!--
Beat pause. Pivot into post-episode work. The audience hasn't seen any of the rest yet. (Phrasing alternative per spec Open Question #4: "A week ago" — more arresting but loses the Nov-2025 → Apr-2026 framing the audience expects from the episode reference.)
-->

---
---

# The workflow

Spec → Plan → Commits.

```text
docs/superpowers/specs/
  2026-04-20-auth-hardening-design.md
  2026-04-20-ai-provider-toggle-design.md
  2026-04-20-user-dietary-profiles-design.md
docs/superpowers/plans/
  2026-04-20-auth-hardening.md
  2026-04-20-ai-provider-toggle.md
  2026-04-20-user-dietary-profiles.md
```

<div class="pt-4 text-base opacity-70">
Each feature: a written spec, a step-by-step plan, then small narrated commits.
</div>

<!--
Describe the loop, don't oversell. The artifacts are real and on disk in the pantry-party repo. Shiki will syntax-color the directory tree as a `text` block — fine.
-->

---
---

# Three features in a week

<div class="grid grid-cols-3 gap-6 pt-8 text-base">

<div>

### Auth hardening
<div class="opacity-60 text-sm pt-1">spec · plan · ~14 commits</div>
<div class="pt-3 opacity-90">Proper auth, env-driven config, deduplicated provider wrapper.</div>

</div>

<div>

### AI provider toggle
<div class="opacity-60 text-sm pt-1">spec · plan · ~16 commits</div>
<div class="pt-3 opacity-90">Claude + OpenAI, switchable per room, provider badge on cards.</div>

</div>

<div>

### Dietary profiles
<div class="opacity-60 text-sm pt-1">spec · plan · ~9 commits + 1 docs</div>
<div class="pt-3 opacity-90">Persistent user preferences, merged into recipe prompts.</div>

</div>

</div>

<div class="pt-10 text-sm opacity-60 text-center">All on 2026-04-20.</div>

<!--
Three feature ships in a week. Pattern, not anecdote. The "all on 2026-04-20" line is true and a little funny — the work was concentrated. (Verify exact commit counts pre-talk from `git log upstream/main` on the pantry-party repo.)
-->

---
---

# Feature 1: Auth hardening

Spec listed four goals: route protection, backend auth re-enabling, provider dedupe, env-driven config.

````md magic-move
```ts
// src/middleware.ts — what we spec'd first
import { clerkMiddleware, createRouteMatcher } from "@clerk/astro/server";

const isProtectedRoute = createRouteMatcher(["/room(.*)", "/create-room"]);

export const onRequest = clerkMiddleware((auth, context, next) => {
  if (isProtectedRoute(context.request) && !auth().userId) {
    return auth().redirectToSignIn();
  }
  return next();
});
```

```tsx
// src/components/AuthGate.tsx — what we shipped instead
import { useAuth } from "@clerk/astro/react";
import { useEffect } from "react";

export default function AuthGate({ children }) {
  const { isLoaded, isSignedIn } = useAuth();
  useEffect(() => {
    if (isLoaded && !isSignedIn) (window as any).Clerk?.openSignIn();
  }, [isLoaded, isSignedIn]);
  if (!isLoaded) return <div>Loading...</div>;
  if (!isSignedIn) return <div>Please sign in to continue.</div>;
  return <>{children}</>;
}
```
````

3 of 4 shipped clean. Route protection backed off — `redirectToSignIn()` is a full-page redirect that broke our modal sign-in UX. Swapped to a client-side gate.

<!--
Two-beat. The magic-move tells the truth: spec'd middleware → shipped AuthGate. The workflow surfaced a real constraint at execution time, not at design time. Don't gloss the rollback — that's the most interesting beat in the deck.
-->

---
---

# Feature 2: AI provider toggle

A per-room switch between OpenAI and Claude.

````md magic-move
```ts
// convex/schema.ts
rooms: defineTable({
  // ...
  aiProvider: v.optional(
    v.union(v.literal("openai"), v.literal("claude")),
  ),
});
```

```ts
// convex/recipeGeneration.ts
const provider = room.aiProvider ?? "openai";
const recipes = provider === "claude"
  ? await generateWithClaude(prompt)
  : await generateWithOpenAI(prompt);
```
````

Schema field · `@anthropic-ai/sdk` · UI toggle · provider badge on each recipe card.

<div class="pt-4 text-sm opacity-60 italic">
We used Claude Code to add Claude support.
</div>

<!--
Most demo-friendly feature — the audience will see the badges flip live in the demo. Meta-aside lands in one breath; don't dwell.
-->

---
---

# Feature 3: Dietary profiles

Persistent user preferences merged into recipe prompts.

- New `userProfiles` Convex table
- Modal triggered from the navbar
- Locked chips in the constraints form (with overrides)
- Union-merged into prompts at generation time

<v-click>

<div class="pt-6 text-base opacity-80 max-w-3xl">
Mid-feature, we hit a <code>@clerk/astro</code> multi-island bug — each React island wrapping its content in <code>&lt;ClerkProvider&gt;</code> from <code>@clerk/clerk-react</code> spawned a duplicate Clerk instance. Real work has bugs.
</div>

</v-click>

<v-click>

<div class="pt-4 text-base">
Switched to <code>useAuth</code> from <code>@clerk/astro/react</code>. Then we wrote it up:
</div>

<div class="pt-2 font-mono text-base">
docs/clerk-astro-react-islands.md
</div>

<div class="pt-2 text-sm opacity-70 italic">
Bug → fix → docs commit. The next person who hits this skips the loop.
</div>

</v-click>

<!--
Lead with calibration ("real work has bugs"). Close with the docs-commit punchline. The integration guide is a real artifact — pantry-party commit 581b1c7. Two click reveals so the calibration lands before the punchline.
-->

---
layout: center
---

# What this changed

<div class="text-3xl py-8 max-w-3xl mx-auto">
Prompt → diff is faster than scaffold → wire.
</div>

<div class="text-base opacity-60">
More time designing. Less time plumbing.
</div>

<!--
Single observation. Don't make it a thesis — the features have already done the arguing. (Final wording per spec Open Question #4: alternatives are "The shift wasn't model IQ; it was process rigor." or "More time designing, less time plumbing." Pick before the talk.)
-->

---
layout: center
---

<script setup>
import QrcodeVue from 'qrcode.vue'
const roomUrl = 'https://pantryparty.lol/room/XXXX'
</script>

# Try it yourself

<div class="grid grid-cols-2 gap-12 items-center pt-4">
  <div>
    <div class="w-72 mx-auto rounded bg-white p-4 flex items-center justify-center">
      <QrcodeVue :value="roomUrl" :size="256" level="H" />
    </div>
    <div class="text-center pt-4 text-xl font-mono">
      {{ roomUrl.replace('https://', '') }}
    </div>
    <div class="text-center pt-2 text-sm opacity-60">
      Sign up (username / password), join the room, add ingredients.
    </div>
  </div>
</div>

<!--
Read the URL out loud. Give the audience ~30s to scan. Then start driving: add pre-seed ingredients, trigger recipes, vote live. The iframe reflects the live site so they can see real-time updates even before they've signed up.

Climax: mid-demo, flip the AI provider toggle. Generate again with the same ingredients. Audience sees the provider badges on the new cards. This is the moment that makes the post-episode work tangible — the thing they'd otherwise have to take our word for.

If either provider is misbehaving (smoke-tested ~1h before talk per rehearsal plan), narrate around it: "we'd flip the toggle here — here's what it would have looked like" and show the backup clip if needed (overview → slide 16).
-->

---
layout: center
---

# Thanks

<div class="pt-4 text-lg space-y-2">

**Pantry Party** — [pantryparty.lol](https://pantryparty.lol)

**Source** — github.com/austingalyon/pantry-party

**This deck** — [deck URL — fill in post-deploy]

</div>

<div class="pt-12 text-sm opacity-60">
  Ryan · Austin · NorfolkJS 2026
</div>

<div class="pt-8 text-3xl">Questions?</div>

<!--
Thanks + Q&A. Leave the QR from slide 18 on screen if audience is still signing up.
-->

---
hideInToc: true
---

# Demo backup

<video src="/video/demo-backup.mp4" controls class="w-full max-w-4xl mx-auto" />

<!--
Hidden from the TOC; sits after the wrap slide so normal flow doesn't land on it. If the live demo fails, press `o` to open the slide overview and jump here, or type the slide number (21) + Enter. 30-second pre-recorded end-to-end run.
-->
