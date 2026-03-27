Date: 2026-03-09
Type: Tech Debt Review
Topic: Search Latency Bottleneck

## Problem
- The current vector search is taking ~4000ms (P99).
- Root Cause: We are re-calculating metadata filters *after* the ANN (Approximate Nearest Neighbor) search. This is inefficient for large tenants like GlobalBank.

## Proposed Solution (RFC)
- Move to **Pre-filtering** approach.
- Use HNSW index with partition keys based on TenantID.
- This will reduce search space by 99% before we even calculate distance.

## Risks
- Re-indexing takes 48 hours. We need to do this on a weekend.
- Memory usage might spike by 20% due to partition overhead.

## Action Items
- Dave: Benchmark HNSW vs IVF-Flat on staging.
- Lisa: Write migration script for existing indices.

---

## 相关笔记

- [[Incident_Postmortem_GlobalBank_Outage]] — 本 RFC 是针对这次 P0 事故根因的技术方案
- [[Architecture_Defense_Python_vs_Rust]] — 同期关于技术栈方向的争论
- [[05_Jira_Ticket_Export_Bug]] — 同一客户 GlobalBank 的另一个性能问题
- [[07_Document_Q1_OKRs_Final_Review]] — OKR 明确要求搜索延迟 <800ms
