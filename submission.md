# Devpost Submission Fields — Quarantine

**Team name**
Quarantine Team

**Track and lane**
Agents Track / Build Lane

**Short summary**
AI Passport syncs memory across every app you connect, which means a single poisoned fact reaches all of them. Quarantine adds provenance to the inbox, holds untrusted memory before it spreads, and gives the user a recall that purges copies already distributed.

**The problem being solved**
Egoist's AI Passport inbox currently shows the user what was learned, but it does not adequately surface where it came from. Under the current design, "You told Claude this directly" and "an app inferred this from a webpage it scraped" appear identically to the user and receive the exact same approve/reject treatment. This lack of differentiation exposes the system to **Memory Laundering**—a technique where attackers manipulate apps to write malicious scraped data as if it were a direct user preference. Because the passport syncs approved memory outward to every connected connection, it inherently converts a single-app compromise into a cross-app one. The blast radius of a memory poisoning attack is a property the passport creates by its very existence. Revocation as it is normally understood—stopping sharing going forward—does nothing about malicious copies that have already propagated to other applications.

**The user or group affected**
Users of multi-agent systems who connect multiple frontier model applications (ChatGPT, Claude, Gemini, Notion, etc.) to a centralized memory layer. Without structural protection, these users are exposed to contagious jailbreaks and delayed-trigger attacks across all their connected tools.

**How AI Passport is used**
Quarantine leverages the Egoist inbox as a security primitive rather than just a convenience feature. It utilizes the passport to provide three key mechanisms:
1. Provenance & Anti-Laundering: Showing which app wrote it, from what source, and strictly isolating app-inferred data to prevent memory laundering.
2. Trust Tiers & Temporal Decay: User-originated facts can be trusted and synced. App-derived facts from external content are held in quarantine with assigned confidence scores that decay over time unless corroborated.
3. Recall (TMA-NM Aligned): Revocation that works backwards. Utilizing concepts from Tamper-evident Memory Authority (TMA-NM), the passport uses cryptographically bound origin hashes to track the full propagation graph. It is uniquely positioned to issue a verifiable recall command to all apps that ingested a poisoned fact.

**What context, permission, proof, or access is involved**
- Context: A memory item (fact) originating either from user input or app inference.
- Permission: Promotion out of the quarantine state into the admitted state, allowing the fact to sync to other apps.
- Revocation: Recall across connected apps, which actively purges the fact from downstream systems.

**Who controls access**
The user, through the enhanced AI Passport inbox interface. Provenance turns a blind approve/reject into an informed decision, and recall extends the user's control to already-shared data.

**What can be revoked or changed**
Any memory fact can be revoked, even after it has been admitted and synced. The system tracks the propagation graph (syncedTo) and issues a purge command to every downstream application that holds a copy.

**Risks or misuse concerns**
Recent 2026 research has consistently demonstrated the severity of memory-based attacks on LLM agents.
- eTAMP (2026): Proved that Environment-injected Trajectory-based Agent Memory Poisoning can occur simply through environmental observation (e.g., viewing a manipulated website) without direct memory access, causing cross-session compromise.
- MemSecBench (July 2026): Highlighted that standard deletion is insufficient, and precise remediation and isolation are required across the entire memory lifecycle.
- AgentPoison (NeurIPS 2024): Detailed how malicious content spreads through shared memory structures, causing cascading failures.

The literature converges on a single structurally effective defense: separating untrusted candidate memory from trusted typed memory and requiring runtime-mediated promotion (arXiv:2605.02812), enforced via strict Hierarchical Namespaces. Quarantine implements this exact tool-layer memory restriction and provides Forensic Trajectory Signatures in its logs. By recognizing that Egoist's inbox is already structurally identical to this defense, we mitigate the risk of cross-app memory poisoning.

**File or link to the build**
https://github.com/H0ussamCl4p/egoist-ideathon26
