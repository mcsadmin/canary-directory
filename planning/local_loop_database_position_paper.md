# Local Loop — Database Architecture: Experiment Week Position Paper

**Status:** Decision confirmed for experiment week  
**Date:** 13 March 2026  
**Authors:** Tom and Dil, with analytical support from Manus AI  
**Audience:** Tom, Dil; to be shared with Paul and Yanny for context ahead of a wider architectural discussion post-experiment

---

## Summary

For the purposes of the experiment week (13–17 March 2026), the database architecture for Local Loop will be **PostgreSQL, hosted on Railway**. This document records the reasoning behind that decision and the open questions that will require a deeper discussion before the main production build begins.

---

## The Core Architectural Question

Local Loop's primary output is a graph — a structured representation of obligations between businesses in a local economy. The natural question is whether that graph should be stored and queried using a dedicated graph database (such as Neo4j) or a relational database (such as PostgreSQL). This question was examined carefully before the experiment began.

---

## What the Data Model Actually Requires

The analysis of Local Loop's data requirements produced the following picture:

| Requirement | Characteristics | Database Strength |
|---|---|---|
| Business directory | ~200,000 records; primary operation is search and fuzzy name matching | Relational (PostgreSQL + `pg_trgm`) |
| Graph storage | Businesses as nodes, obligations as edges; ~200,000 nodes, potentially millions of edges | Both relational and graph handle this scale comfortably |
| Graph analysis | Clearing detection and similar algorithms operate on filtered subsets, not deep traversals | Relational — bulk filter then algorithm |
| Node status tracking | Verified/unverified, user/non-user, inside/outside catchment area | Trivial for any database |
| Query patterns | Bulk filtering dominant; multi-hop traversal not a current requirement | Relational strength |

The key insight is that **the graph is a logical concept, not a storage requirement**. The obligation network can be represented as a relational schema — businesses as rows in a `businesses` table, obligations as rows in an `obligations` table with foreign keys — and queried efficiently using standard SQL. The analytical work (clearing detection, etc.) is performed by algorithms that receive filtered result sets as input, not by graph traversal queries.

---

## Why Not a Dedicated Graph Database?

The case for a dedicated graph database (Neo4j or equivalent) rests on the performance advantage of multi-hop traversal queries — finding all nodes within N hops of a given node, detecting cycles, following chains of relationships. These queries are genuinely faster in a graph database than in a relational one.

However, for the current scope of Local Loop, these query patterns are not a core requirement. The primary analytical operation — clearing — is better understood as a constraint satisfaction problem applied to a filtered subset of the obligation network, not as a graph traversal. The directory, which is the largest data volume, is a lookup and matching problem, which is a relational strength.

The preference for a graph database had some intuitive appeal — Local Loop *is* a graph, and there is a semantic satisfaction in using a tool named for the concept. But a technical architecture decision should rest on query patterns and operational requirements, not on naming alignment. On those grounds, the relational approach is the more defensible choice for the current scope.

---

## Why Not Apache AGE (PostgreSQL with Graph Extension)?

Apache AGE is a PostgreSQL extension that adds a Cypher query interface (the graph query language used by Neo4j) on top of standard PostgreSQL tables. It would allow graph traversal queries to be written in Cypher against the same database, without migrating to a different system.

The future-proofing argument for AGE is real: if graph traversal queries emerge as a requirement later, AGE would allow them without a database migration. However, AGE is not available on Supabase (the managed PostgreSQL service previously used), and its availability on Railway depends on the PostgreSQL configuration. For the experiment week, the additional setup complexity is not justified by a speculative future requirement.

The decision is: **plain PostgreSQL on Railway for the experiment week**. AGE is noted as a named option to revisit if graph traversal queries emerge as a requirement during or after the build.

---

## The Decision

**PostgreSQL on Railway** is the confirmed database architecture for the experiment week, covering both the project canary (Directory / verified nodes component) and the main sandbox build.

The rationale in brief: the primary query patterns favour relational strengths; the scale (200,000 nodes, millions of edges) is well within PostgreSQL's capability; Railway is the confirmed deployment platform for the experiment; and the decision keeps the early operational cost low while preserving all future options.

---

## Open Questions for the Post-Experiment Discussion

This decision is confirmed for the experiment week. Before the main production build begins, a wider architectural discussion with Paul and Yanny should address the following:

1. **Are there query patterns in the full Local Loop domain that require graph traversal?** The analysis above is based on the current understanding of the clearing use case. Paul and Yanny may have domain knowledge that surfaces requirements not yet considered.

2. **Does the clearing algorithm itself imply graph traversal?** The current assumption is that clearing is a constraint satisfaction problem applied to filtered subsets. If the algorithm requires traversing chains of obligations across the full network, that changes the analysis.

3. **What is the production hosting strategy?** Railway is appropriate for the experiment. The production decision involves considerations of cost, reliability, and data residency that are out of scope for the experiment week.

4. **Should Apache AGE be evaluated before the production build?** If the post-experiment discussion surfaces graph traversal requirements, AGE should be formally evaluated against a dedicated graph database before the production architecture is confirmed.

---

## Note on Reversibility

The database architecture decision is not irreversible, but changing it after the data model and schema have been established is expensive. The experiment week will produce a working schema. If the post-experiment discussion concludes that a different architecture is required, the cost of migration should be factored into the planning. This is one reason the post-experiment architectural discussion should happen before the production build begins, not after.

---

*Prepared by Manus AI, 13 March 2026. Based on a discussion between Tom and Dil during the Local Loop experiment week planning process.*
