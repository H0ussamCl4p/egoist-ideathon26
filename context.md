# Context — AI Passport Ideathon

Read this first. It explains what the competition is, what the product is, what we
decided to build, and why. `plan.md` covers how to build it.

---

## 1. The event

**AI Passport Ideathon**, hosted by Egoist Machines (YC S26) on Devpost.

| | |
|---|---|
| Runs | August 7–12, 2026 |
| Deadline | **August 12, 2026, 11:45pm GMT+1** |
| Participants | ~68, invite only |
| Format | Online, individual or team |
| Host | Erin McGurk, co-founder & CEO, Egoist Machines |

Pick **one track** (Work, Creative, Agents, Identity) and **one lane** (Build or
Concept). Prizes are per track, plus one internship interview prize with the
YC-backed team.

The host's framing on LinkedIn was explicit: this is not about coding skill. It is
about creativity, commercial instinct, and genuinely exciting ideas. Treat the code
as a vehicle for the argument, not the deliverable itself.

### Required submission fields

The form asks for each of these. Every one needs a real answer:

- Team name
- Track and lane
- Short summary
- The problem being solved
- The user or group affected
- How AI Passport is used
- What context, permission, proof, or access is involved
- Who controls access
- What can be revoked or changed
- Risks or misuse concerns
- File or link to the build

### Judging criteria (published)

1. **Privacy and risk** — avoids collecting more than needed; names the main risks
   and explains how the design reduces them.
2. **User control** — shows how the person stays in control: who can access what,
   what can be changed, what can be revoked.
3. **Clear AI Passport use** — exactly what context, permission, proof, or access is
   being carried, checked, granted, or revoked.

The rules page adds: a real problem, a specific user, a reason the solution must
travel across tools, a privacy-aware design, and a clear first version.

Note what is *absent* from the rubric: technical sophistication, scope, and feature
count. Nothing rewards building more. Narrowness is the strategy.

---

## 2. What AI Passport actually is

Getting this precise matters, because most entries will pitch things the product
does not do.

Egoist Machines is a two-person YC Summer 2026 company (Erin McGurk, CEO; Dr. David
Khachaturov, CTO — Cambridge PhD in ML security, ex-OpenAI). AI Passport is a
**user-owned memory and preference layer that travels between AI apps**.

How it works, per the company's own descriptions:

1. A user creates a passport at ego.ist.
2. They connect apps — ChatGPT, Claude, Gemini, Gmail, calendar, Notion, GitHub,
   wearables.
3. When a user adds information in a connected app, it syncs to the user's **inbox**.
4. The user reviews the inbox and decides per item: share, keep private, or delete.
5. Connected apps can *request* specific information (e.g. travel preferences).
6. Sensitive fields (e.g. date of birth) are never shared without explicit permission.

There is also a public card at `ego.ist/i/you`, positioned as a free Linktree
alternative — consumer distribution on top of the private memory graph.

### The primitives we actually get to use

- **Approved context** — facts the user has promoted into the passport
- **Scoped app requests** — an app asking for a specific category
- **Revocation** — withdrawing access
- **Provenance** — where a piece of context came from

### What the passport is NOT

It is not a verifiable-credentials wallet. There is no issuer, no attestation, no
cryptographic proof of external claims. Any idea that depends on a university,
employer, or government *signing* something is describing a different product.

---

## 3. Our entry

**Track: Agents. Lane: Build.**

**Name: Quarantine** — provenance, containment, and recall for AI Passport.

### One-line summary

AI Passport syncs memory across every app you connect, which means a single poisoned
fact reaches all of them. Quarantine adds provenance to the inbox, holds untrusted
memory before it spreads, and gives the user a recall that purges copies already
distributed.

### The problem

Egoist's inbox currently shows the user *what* was learned. It does not show *where
it came from*. "You told Claude this directly" and "an app inferred this from a
webpage it scraped" appear identically and get the same approve/reject treatment.

Because the passport syncs approved memory outward to every connection, it converts a
single-app compromise into a cross-app one. The blast radius is a property the
passport creates by existing. And revocation as normally understood — stop sharing
going forward — does nothing about copies that already propagated.

### The research this rests on

This is the part that makes the entry defensible, and the reason to cite sources in
the submission rather than asserting.

- **MINJA (Dong et al., 2025)** — memory injection into LLM agents through ordinary
  query-only interaction, no elevated privileges required. >95% injection success
  rate. arXiv:2503.03704
- **Follow-up evaluation (2026)** — arXiv:2601.05504, memory poisoning attack and
  defense on memory-based LLM agents, tested on clinical agents.
- **Delayed-trigger attacks / Hidden in Memory (2026)** — frontier models store
  adversary-induced memories at ~99.8% injection rate; five of six defense classes
  fail; only **tool-layer memory restriction** provides structural protection.
  (Discussed in arXiv:2606.30566)
- **AgentPoison (NeurIPS 2024)** and contagious-jailbreak work — malicious content
  spreads through *shared* memory structures, causing cascading failures across
  multi-agent systems.
- **Agent worm research (2026)** — arXiv:2605.02812 — the recommended architecture is
  "separating untrusted candidate memory from trusted typed memory and requiring
  runtime-mediated promotion rather than free-form memory writes."

That last one is the key: **the defense the literature converges on is structurally
identical to Egoist's inbox.** Egoist has already built the right security
architecture and is currently marketing it as a convenience feature. Our entry names
that and completes it.

### The three mechanisms

1. **Provenance on every inbox item.** Show which app wrote it, from what source, and
   whether the origin was the user directly or content the app ingested.
2. **Trust tiers instead of flat approve/reject.** User-originated facts can be
   trusted. App-derived facts from external content are quarantined by default and
   never auto-sync. This is the tool-layer restriction the research identifies as the
   only structurally effective defense.
3. **Recall.** Revocation that works backwards. The passport is the only system that
   knows the full propagation graph, so it is the only thing that can issue a recall:
   this fact was poisoned, here are the four apps that ingested it, purge it.

Mechanism 3 is the answer to "why must this travel across tools" — no single app can
do it. Only the passport can.

### How it maps to the rubric

| Criterion | Our answer |
|---|---|
| Privacy and risk | A documented attack class with published success rates, plus an architecture that reduces it |
| User control | Provenance turns a blind approve/reject into an informed decision; recall extends control to already-shared data |
| Clear AI Passport use | Context = a memory item. Permission = promotion out of quarantine. Revocation = recall across connected apps. |

### Tone

Frame this as completing Egoist's own architecture, not criticizing it. The message
is "your inbox is a security primitive and you are underselling it." That reads as
insight to a founder. "Your product is unsafe" reads as an attack.

---

## 4. What not to do

- Do not build a working passport. No backend, no auth, no real app connections.
- Do not pitch AI content licensing — RSL 1.0 became an official industry standard in
  December 2025 with 1500+ endorsing organizations. That space is taken.
- Do not pitch agent authorization protocols — IETF drafts and the MCP authorization
  spec already cover delegation, audience binding, and audit trails.
- Do not pitch proof-of-personhood — crowded, well funded, and the passport cannot
  issue attestations anyway.
- Do not widen scope to look impressive. The rubric does not reward it.
