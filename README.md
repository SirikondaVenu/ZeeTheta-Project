# PayScale HTTPE — 15-Day Architecture Submission

**Project:** High-Throughput Transaction Processing Engine (HTTPE)  
**Company:** PayScale Financial Technologies  
**Author:** Manus AI  
**Status:** Architecture/design submission; load and chaos results are planned, not executed.

## Executive summary

This submission redesigns PayScale from a 1,200 TPS PostgreSQL-single-primary and single-node RabbitMQ system into an India-resident, horizontally scalable transaction platform. The target design uses an API gateway, autoscaled services, CockroachDB-compatible distributed SQL with PostgreSQL wire compatibility, Redis Cluster, a three-AZ Kafka cluster, and explicit correctness controls. The design targets 12,000 TPS sustained, 18,000 TPS for 15 minutes, p50 at or below 30 ms, p99 at or below 100 ms, 99.99% availability, zero committed-data loss, and a planning cost below the $45,000 monthly ceiling.

## Deliverables

- Day01: scenario and bottleneck analysis.
- Day02: technology comparisons and two ADRs.
- Day03: HLD narrative and Mermaid/PlantUML diagrams.
- Day04-06: schema, ERD, SQL, indexing, partitioning, pooling, and sharding.
- Day07-08: Kafka design, topics, idempotency, and OCC pseudocode.
- Day09-10: resilience, circuit breaker, retry, timeout, bulkhead, and fault tolerance.
- Day11-12: OpenAPI, load-test strategy, capacity and cost plan.
- Day13: FMEA with 25 failure modes and five chaos experiments.
- Day14: reflection and ARB presentation content.
- Day15: final report and Q&A.

## Core assumptions

1. The target distributed SQL choice is CockroachDB-compatible infrastructure because it supplies serializable transactions, automatic replication, and PostgreSQL-compatible client behavior. PostgreSQL+Citus remains a viable alternative if operational familiarity is valued above multi-region SQL semantics.
2. India-resident cloud regions, managed services, and backup destinations are available at the estimated planning prices. Pricing is an estimate, not an actual cloud bill.
3. The critical payment commit is synchronous in the ledger database. Notifications, reconciliation, and non-critical analytics are asynchronous.
4. A 24-hour idempotency-key record is retained in the database and Redis is used only as a fast front door, so Redis loss cannot create duplicate payments.
5. “Exactly once” is implemented as effectively-once business processing through durable deduplication and atomic database state transitions; transport-level exactly-once alone is not treated as sufficient.

## How to review

Start with `Day15/Final_Architecture_Report.md`, then inspect the source artifacts referenced by it. The `Day11-12/Capacity_Plan.xlsx` and `Day13/FMEA.xlsx` files contain structured planning tables. The requested native `.pptx` is not generated in this session; `Day14/ARB_Presentation.md` is the complete slide-by-slide deck with speaker notes, and the HTML-mode deck is delivered through the Slides tool.

## Quality status

All required Day 1–15 sections are represented. Numerical targets are kept consistent with the project brief. Test outcomes are not claimed; acceptance criteria and execution plans are clearly separated from actual evidence.
