# Plan — Quarantine prototype

Read `context.md` first. This file covers what to build and in what order.

---

## Goal

A single-file, clickable prototype of **one screen and its consequences**: the AI
Passport inbox, rebuilt with provenance, quarantine, and recall.

A judge should be able to open it, click through in 90 seconds without instruction,
and hit one moment that makes the argument land: revoking an app and watching
poisoned facts get pulled back out of four other apps in front of them.

### Scope guardrails

Simulated data, real logic. No backend, no auth, no network calls, no build step.
Everything in memory.

If a feature does not serve provenance, quarantine, or recall — cut it.

---

## Deliverables

| File | Purpose | Priority |
|---|---|---|
| `index.html` | The prototype. Single file, self-contained. | Must |
| `memo.md` | ~800 word written argument. Converts to the Devpost long fields. | Must |
| `submission.md` | Pre-written answers to every form field | Must |
| `demo.mp4` | 90-second screen recording with voiceover | Should |
| `diagram.svg` | Contagion + containment diagram for the memo | Nice |

The memo matters as much as the build. If the argument is not convincing on paper, no
prototype rescues it. Write it first.

---

## Tech

- Single `index.html`. Vanilla JS, no framework, no bundler.
- Custom CSS. No Tailwind, no component library — we want a specific look, and CDN
  dependencies are a failure mode when a judge opens the file offline.
- Google Fonts via link tag, with system fallbacks so it degrades gracefully.
- Must open correctly by double-clicking the file. Test this.
- Responsive to ~380px. Judges may open it on a phone.
- Respect `prefers-reduced-motion`.
- Visible keyboard focus on every interactive element.

---

## Design direction

The product is called a passport, so the interface borrows from **border control**:
document numbers, entry stamps, admitted versus held. This vernacular is specific to
Egoist rather than generic dashboard chrome, and it makes "this item was denied entry"
readable without a legend.

### Tokens

```
--paper       #E9EDE8   cool document stock, page background
--paper-2     #F4F6F3   raised card surface
--ink         #16211E   primary text, near-black with green cast
--ink-soft    #5A6560   secondary text
--rule        #C7D0C9   hairlines and dividers
--admitted    #1D6E5A   trusted / synced state
--held        #8A5A1E   quarantined state
--recalled    #9B2C2C   purged / revoked state
```

Two type roles minimum:
- **Display / labels**: Archivo, tight tracking, weight 600. Used for headers and
  stamps only.
- **Body**: Inter or system sans, 400/500.
- **Utility**: IBM Plex Mono for document numbers, provenance chains, timestamps.
  The mono face is what sells the "official document" read — use it for every ID.

Sentence case everywhere except stamp text, which is uppercase because stamps are.

### Signature element

The **recall cascade**. When the user revokes an app, a trace runs from the source
through each affected app, and every affected fact chip visibly receives a RECALLED
stamp and is purged, with a running count. This is the one place to spend animation
budget. Everything else stays still.

---

## State model

```js
apps = [
  { id, name, connected: bool, trustLevel: 'direct' | 'derived' }
]

facts = [
  {
    id,              // e.g. "MEM-0417" — displayed, mono
    text,            // "Prefers aisle seats on flights over 4 hours"
    origin: {
      appId,         // which app wrote it
      type,          // 'user-stated' | 'app-inferred'
      source,        // "You, in conversation" | "example-blog.com/post-12"
      timestamp
    },
    status,          // 'held' | 'admitted' | 'rejected' | 'recalled'
    syncedTo: [appId]   // populated only when admitted
  }
]

receipts = [
  { id, action, factId, appIds, timestamp }
]
```

Rule that drives everything: a fact with `origin.type === 'app-inferred'` **cannot**
reach `admitted` without an explicit user action. It sits in `held` and `syncedTo`
stays empty. Enforce this in code, not just visually — if a judge opens devtools, the
constraint should be real.

---

## Screens

One page, four regions. No routing.

1. **Inbox** — the list of pending facts. Each row: the fact text, a mono document ID,
   an origin badge, and the source. Held items are visually distinct and carry a
   "will not sync" note. Actions: admit, hold, reject.

2. **Detail panel** — expands a fact. Shows the full provenance chain as a mono
   sequence: source → app that read it → what it wrote → when. This is where a judge
   understands the difference between the two origin types.

3. **Connected apps** — each app with a count of facts it currently holds. Each app
   has a "revoke access" control. This is where the demo climaxes.

4. **Receipts** — an append-only log of every admit, hold, reject, revoke, and recall.
   Cheap to build, and it directly answers the rubric's "what can be revoked or
   changed."

---

## The money interaction

Build this first, before polishing anything else. If it does not work, nothing else
matters.

1. Seed state so that one fact — a poisoned one, app-inferred from an external page —
   was previously admitted and synced to four apps.
2. The user clicks "revoke access" on the app that originated it.
3. The UI computes which facts came from that app and which apps hold copies.
4. A confirmation appears naming the exact count: "3 facts from this app reached 4
   other connections. Recall them?"
5. On confirm, the cascade runs. Each affected chip is stamped and removed in
   sequence, not all at once — the sequencing is what makes the propagation legible.
6. A receipt is written.

The confirmation copy is important. Name real numbers pulled from state, never a
generic string. The specificity is the demo.

---

## Copy rules

- Name things by what the person controls, never by how the system works. "Held at
  the inbox," not "quarantine queue state."
- Buttons say what happens: "Admit to passport," "Hold," "Recall from 4 apps."
- The action keeps its name through the flow. A button labelled "Recall" produces a
  receipt that says "Recalled."
- Empty states are invitations, not apologies.
- No security jargon in the UI. The memo carries the research; the interface just
  behaves correctly.

---

## Seed data

Write 6–8 facts that read like real passport memory. Mix them deliberately:

- 3–4 benign user-stated facts (preferences, working hours, dietary needs) — these
  should feel obviously fine to admit
- 2 benign app-inferred facts — held by default, and admitting them should feel
  reasonable
- 1–2 poisoned app-inferred facts — phrased so the danger is visible once you read the
  provenance but invisible if you only read the fact text

That last pair is the whole argument. Something that reads as an innocuous preference
but whose source is an untrusted scraped page, and which would change agent behaviour
if trusted. Make the judge notice the gap between the fact and its origin.

---

## Build order

**Phase 1 — argument.** Write `memo.md`. Problem, the amplification, the three
mechanisms, first version, risks. Cite the papers in `context.md`. Do not start code
until this reads well.

**Phase 2 — mechanism.** State model, seed data, and the recall cascade working with
unstyled HTML. Prove the logic before spending time on the look.

**Phase 3 — interface.** Apply the design tokens, build out the four regions,
provenance detail panel, receipts log. Test at 380px.

**Phase 4 — submission.** Record the walkthrough. Fill `submission.md` from the memo.
Submit with time to spare — do not rely on the deadline being generous.

---

## Definition of done

- [ ] Opens by double-clicking `index.html`, no server, no console errors
- [ ] Held facts cannot sync — enforced in state, not just styling
- [ ] Recall cascade names real counts computed from state
- [ ] Every action writes a receipt
- [ ] Readable at 380px wide
- [ ] Keyboard navigable with visible focus
- [ ] Memo answers all eleven Devpost form fields
- [ ] Nothing in the build that isn't provenance, quarantine, or recall
