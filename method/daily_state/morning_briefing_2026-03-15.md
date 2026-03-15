# Morning Briefing — 15 March 2026
### Canary: Directory / Entity Resolution

---

**Where we are.** The canary is in the requirements phase. Session 1 (requirements elicitation) is complete. Session 2 (requirements specification) has a charter agreed but no work has been done yet.

**What we did yesterday and how it went.** We completed the onboarding protocol, verified repository access, and ran Session 1: requirements elicitation. The scope boundary, done criterion, and known constraints were agreed, and five requirements groups were identified and sequenced — Invoice Ingestion, Entity Resolution, Directory State and Integrity, Operator UI and Verification, and Non-Functional Requirements. The requirements brief for Group 1 (Invoice Ingestion, RB-001) was produced and committed. One process error occurred: the coaching agent skipped the Session 2 charter and went straight to producing the brief. This was caught and corrected. The session charter is a mandatory gate — lesson learned.

**What today looks like.**

| Session | What | T-shirt Size |
|---|---|---|
| **Session 2** (charter already agreed) | Requirements specification — five sub-sessions, one per group. Coaching agent produces a brief, you take it to the requirements coaching task, return the artefacts, coaching agent reviews for sufficiency and consistency, then briefs the next group. | 90 min |
| **Session 3** | Final sufficiency review across all groups. If all groups are closed: produce the coding agent handoff brief. | 20 min |
| **Session 4** | Coding agent build session — hand off to Cursor. | 45–90 min |
| **Session 5** | Review and deployment — verify build output, confirm `canary.commoner.services`. | 20 min |

**Flags worth knowing before we start.**

The blind spot check last night flagged one item: **dummy invoice test data**. Nobody has discussed whether a sample dataset exists, needs to be generated, or what "realistic" means in practice. This will likely come up naturally during the Invoice Ingestion requirements session, but it is worth having in mind — the canary's done criterion requires the ability to "input arbitrary but valid sets of invoice data," so the test data question needs an answer before the build is complete.

Railway DNS for `canary.commoner.services` was reported as confirmed yesterday, but the preflight noted it requires a first deployment. Worth a quick check before Session 4.

---

Session 2 is ready to go. RB-001 (Invoice Ingestion) is already committed and waiting. When you are ready, take it to the requirements coaching task and come back with the artefacts.
