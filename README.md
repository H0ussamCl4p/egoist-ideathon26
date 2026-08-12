# Quarantine — Provenance, Containment, and Recall for AI Passport

> **AI Passport Ideathon 2026** (Hosted by Egoist Machines on Devpost)  
> **Track:** Agents | **Lane:** Build  
> **Repository:** [https://github.com/H0ussamCl4p/egoist-ideathon26](https://github.com/H0ussamCl4p/egoist-ideathon26)

---

## 📌 Overview

AI Passport syncs memory across every app you connect, which means a single poisoned fact can reach all of them. **Quarantine** adds provenance to the Egoist inbox, holds untrusted memory before it spreads, and provides a **verifiable recall cascade** that purges copies already distributed to downstream applications.

---

## 🚀 Key Features

1. **Provenance on Every Inbox Item**  
   Exposes which app wrote the memory, the exact origin (direct user input vs. web scraping), TMA hash signatures, and trajectory forensic status.
   
2. **Trust Tiers & Temporal Decay**  
   - **User-stated facts**: Trusted and eligible to sync to connected tools.
   - **App-inferred facts**: Quarantined by default, assigned decaying confidence scores, and restricted from auto-syncing without explicit permission.

3. **Verifiable Recall Cascade**  
   Revocation that works backwards. When access is revoked for an app, Quarantine uses cryptographically bound origin hashes to track propagation and automatically purges all downstream copies.

4. **System Receipts Log**  
   An append-only audit trail logging every `ADMITTED`, `HELD`, `REJECTED`, `REVOKED`, and `PURGED_CASCADE` event.

---

## 🏃 How to Run

This prototype is built as a zero-dependency, single-file web application.

1. Clone or download this repository.
2. Double-click `index.html` or open it in any modern web browser (Chrome, Firefox, Edge, Safari).
3. No server, node packages, or build tools are required.

---

## 📚 Research Grounding

- **eTAMP (2026)**: Environment-injected Trajectory-based Agent Memory Poisoning demonstration.
- **MemSecBench (July 2026)**: Evaluation proving standard deletion is insufficient for multi-agent memory safety.
- **AgentPoison (NeurIPS 2024)**: Cascading memory attack dynamics in multi-agent networks.
- **Tool-Layer Restriction (arXiv:2605.02812)**: Recommends separating untrusted candidate memory from trusted typed memory—the core architecture Quarantine enforces.

---

## 📄 Submission Files

- `index.html`: Interactive prototype.
- `submission.md`: Pre-formatted Devpost submission form responses.
- `memo.md`: Written argument & detailed breakdown.
- `context.md`: Ideathon rules, rubric, and product background context.
- `plan.md`: Prototype design specifications.

---

## ⚖️ License & Copyright

Made with ❤️ by **Choubik Houssam** &copy; 2026. All rights reserved.
