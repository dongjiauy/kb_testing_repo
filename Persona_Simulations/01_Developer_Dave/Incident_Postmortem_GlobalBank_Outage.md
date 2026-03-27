# Incident Postmortem: GlobalBank Outage (P0)
**Date**: 2026-03-12
**Duration**: 4 hours 23 mins
**Impact**: GlobalBank compliance team unable to access documents. CEO called Li Zong screaming.

## Timeline
- **09:00**: Alert triggered. API Latency > 10s.
- **09:15**: Auto-scaling group maxed out at 50 pods. CPU usage 100%.
- **09:30**: Dave logs in. Suspects DDoS.
- **10:00**: Traffic analysis shows it's internal. A single query from GlobalBank is triggering a Full Table Scan on the vector DB.
- **11:00**: Identified the query: "Find all documents related to risk." (Too broad).
- **12:00**: Hotfix deployed. Added query limiter and forced metadata filtering.

## Root Cause
- **Memory Leak**: The new Python bindings for the vector engine were not releasing memory when handling result sets > 10k items.
- **No Guardrails**: We allowed users to run `SELECT *` equivalent queries on vector space.

## Action Items
- [x] Fix memory leak in `vector_engine.rs` (Dave).
- [ ] Implement strict query timeout (5s) (Lisa).
- [ ] Refund GlobalBank 1 month of service credits (Sales).

---

## 相关笔记

- [[RFC_Vector_Search_Optimization]] — 针对本次事故根因提出的技术 RFC
- [[05_Jira_Ticket_Export_Bug]] — GlobalBank 的另一个 P0：PDF 导出超时
- [[Code_Review_Notes_Security]] — GlobalBank 安全审计前的代码安全问题
- [[02_Document_Series_B_Pitch_Deck_Draft]] — GlobalBank 是 Pitch Deck 的核心客户案例
