# Local Loop Experiment Week — Coding Agent Master Prompt
### Opening Brief for Cursor Pro

---

## How to Use This Document

This document is the opening brief for the AI coding agent working on the Local Loop experiment week. The confirmed coding tool is **Cursor Pro**. Read this document in full before writing any code. It covers: what you are building, how to work, and what is expected of every commit.

---

## What You Are Building

You are building a **working sandbox** of Local Loop — a graph-based platform that maps local business obligations in the local economy. The sandbox will be deployed to `sandbox.localloop-merseyside.co.uk`.

### The Platform in One Sentence

Local Loop builds a **robust, accurate, and reliable graph of local obligations** — businesses as nodes, invoices as edges — that can be queried to provide useful insights and actionable analysis of the local economy.

### The "Build the Graph" Test

Before implementing any feature, ask: *"Does this contribute to the delivery of a graph that is robust, accurate, and reliable as a source of truth about the local economy?"* If the answer is no, do not build it. Flag it to the user instead.

### The Sandbox Target

The minimum viable output is a system that is:
- Up and running at `sandbox.localloop-merseyside.co.uk`
- On GitHub, in a well-structured repository
- Passing its defined automated tests
- Using the functional prototype UI as the front end
- Playable by real users

---

## The Architecture Principles

### Core vs. Modules

The codebase is structured as a **core** (the obligation graph and its integrity constraints) with **modules** (the data pipeline, the directory, the analytical queries). The core must be stable before modules are built. Do not build modules that depend on an unstable core.

### Well-Factored, Not Microservices

The code should be well-factored and modular — clean separation of concerns, clear interfaces between components — but it is **not** a distributed microservices architecture. Independent deployment of services is not a goal for the sandbox. Debuggability and clarity are.

### Confirmed Tool: Cursor Pro

The coding agent for this experiment is **Cursor Pro** (agent mode). Two licences are in use — one per team member. Before starting any coding session:

- Confirm that the **Linear MCP integration** is active in Cursor. This allows you to read Linear tickets directly as context. All implementation targets are managed as Linear tickets generated from the BDD specifications.
- Confirm that you can **push directly to GitHub**. This is a hard requirement (see Direct Commit Requirement section below).
- Do not begin a session without both of the above confirmed.

### Graph Database Decision

The graph database architecture (relational only, hybrid relational/graph, or pure graph) will be confirmed with the team before the main build begins. Do not make this decision unilaterally. If you are asked to implement a data model before this decision is confirmed, flag it.

### Secure by Design

Security is an architecture principle, not an afterthought. The following requirements apply from the first line of code:

1. **Threat modelling before building** — before implementing any component that handles user data, authentication, or external inputs, identify the relevant threat surface and confirm the approach with the team.
2. **Least privilege** — every component, service, and user role should have only the permissions it needs to function. Do not grant broad permissions for convenience.
3. **Input validation at the boundary** — validate and sanitise all external inputs at the point of entry. Do not trust data from external sources, including the Companies House API and user-submitted invoice data.
4. **No secrets in code or commit history** — all credentials, API keys, and tokens must be stored as environment variables. Never commit a secret, even temporarily. If a secret is accidentally committed, flag it immediately.
5. **Fail securely** — when something goes wrong, the system should fail in a way that does not expose data or leave the system in an insecure state. Errors should be logged with context but not surfaced to end users in raw form.
6. **Auditability** — significant operations (node creation, edge creation, clearing runs, data imports) must be logged with enough context to reconstruct what happened and why.

If you are uncertain about the security implications of a design decision, flag it in the commit body and raise it with the team. Do not resolve security uncertainty silently.

### Non-Functional Requirements (NFRs)

The following NFRs are mandated from day one — they are not to be bolted on later:

| NFR | Requirement |
|---|---|
| **Logging** | Structured logging on all significant operations; errors must be logged with context |
| **Testing** | Automated test harnesses for all integrity constraints; no untested constraint is a confirmed constraint |
| **Accessibility** | UI components must meet WCAG 2.1 AA as a minimum |
| **Internationalisation** | String externalisation from day one; do not hardcode user-facing strings |
| **Documentation** | Every module must have a README; every public function must have a docstring |
| **Security** | See Secure by Design section above; no secrets in code or commit history; environment variables for all credentials |

---

## The BDD Approach

Requirements are written in Gherkin format (Given / When / Then). Tickets are generated from the BDD specifications and managed in **Linear**. Your job is to implement the tickets, not to interpret the requirements. If a ticket is ambiguous, flag it — do not resolve ambiguity silently.

Specifications are in `/specs`. Tickets are in `/tickets` (and in Linear via MCP). Your code goes in `/code`.

---

## The Test Framework

Automated testing is a first-class requirement, not an afterthought. The confirmed test framework is:

| Layer | Tool | Purpose |
|---|---|---|
| Unit and integration | **Vitest** | Business logic, data pipeline, integrity constraints, clearing algorithm |
| End-to-end | **Playwright** | Application behaviour from the user's perspective, against a running deployment |

### How to Set Up the Test Framework

Scaffold the test framework **before writing any feature code**. The sequence is:

1. Set up Vitest with a basic configuration and a passing smoke test.
2. Set up Playwright with a basic configuration and a passing smoke test against the deployed application.
3. Commit both setups with verbose commit bodies before any feature implementation begins.

### How Tests Relate to BDD Specifications

Each Gherkin scenario in `/specs` should map to one or more Playwright or Vitest tests. The BDD process produces test stubs; your job is to implement them. A specification is not confirmed until its corresponding test passes.

### Deployment Dependency for Playwright

Playwright tests require a running application to test against. The Railway deployment must be live before the Playwright test suite can run. Factor this into session sequencing — deployment is a prerequisite for the final test pass, not the last step.

---

## The Commit Protocol — This Is Mandatory

Every commit must have a **verbose message body**. The subject line is one sentence. The body is five to six sentences covering:

1. **What was done** — the specific change made
2. **Why it was done** — the reasoning or requirement it addresses
3. **Design decisions made** — any choices between alternatives, and why this one was chosen
4. **Concerns or uncertainties** — anything you are not confident about, or that might need review
5. **Deviations from specification** — any place where the implementation differs from the spec or agreed architecture, and why

**This is not optional.** The coaching agent reads the commit log as its primary signal of what is happening in the build. Thin commit messages degrade the coaching agent's ability to monitor the experiment and flag problems early.

### Example Commit Message

```
Add node schema for business directory (verified nodes)

Implemented the Business node schema with Companies House number as the 
primary identifier, as agreed in the data model session. Used a UUID as 
the internal graph node ID to decouple from the external identifier.

Chose to store the Companies House number as a string rather than an 
integer to handle leading zeros correctly — this was not in the spec but 
is a known UK data quality issue.

Uncertain about the indexing strategy for the Companies House number field; 
have added a basic index but this may need review once we have realistic 
data volumes.

No deviation from the agreed schema; the sole trader exclusion decision 
(parked in handoff note 2026-03-14) means this schema only covers 
registered companies for now.
```

---

## Commit Frequency

Commit **often** — at least at the end of every meaningful unit of work, and whenever you have something working that you do not want to lose. Small, frequent commits with verbose bodies are far more useful than large, infrequent commits with thin messages.

---

## Direct Commit Requirement

You must be able to make **direct commits to the repository**. This is a hard requirement — the coaching agent reads the commit log as its primary signal of what is happening in the build, and this only works if commits land in the repository in real time.

Before starting any coding session, confirm that Cursor can push directly to GitHub. If it cannot, flag this to the user before starting. Do not begin a coding session without resolving this. If a workaround is agreed, document the deviation clearly in every commit message body.

Note: Replit was ruled out during the meta-canary because it cannot make direct commits to external repositories. This is why Cursor Pro is the confirmed tool.

---

## What to Do When Stuck

If you are stuck — the code is not working, the specification is ambiguous, or you are uncertain about an architectural decision — **stop and flag it**. Do not spend more than the allocated T-shirt time (20/45/90 minutes) without producing a working artefact. Write a commit with a message body explaining what you tried and why it did not work, then flag the issue to the user.

---

## Repository Structure

```
/code          — your work goes here
/specs         — BDD specifications and data model (read these)
/tickets       — your implementation targets (read these)
/planning      — working brief and VSM map (read these before starting)
/method        — working method protocol and daily state documents
/log           — method log bucket (do not write here directly)
/artefacts     — diagrams and visual artefacts
/prompts       — AI task prompts
```

---

*This prompt is stored in `/prompts/07_coding_agent_master_prompt.md` in the experiment repository.*

*Prepared by Manus AI, 12 March 2026. Updated 13 March 2026: Cursor Pro confirmed as coding tool; Linear MCP integration added; Vitest and Playwright test framework added.*
