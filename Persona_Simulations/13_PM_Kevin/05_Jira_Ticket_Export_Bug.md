# JIRA Ticket: BE-4092
**Title**: [GlobalBank] PDF Export Timeout
**Priority**: P0 (Critical)
**Assignee**: Dave (Backend) -> Unassigned (Dave quit?)

## Description
When GlobalBank tries to export their "Annual Risk Report" (500 pages), the server times out after 30s.
Nginx returns 504 Gateway Time-out.

## Comments
- **Kevin**: Dave, can we increase the timeout?
- **Dave (Old)**: No. The Python worker runs out of RAM. We need to stream the response.
- **New VP (Kevin)**: Rewrite it in Rust.
- **Kevin**: We need it fixed TODAY. Not in 3 months.
- **Lisa**: I can patch it by increasing the worker memory limit to 8GB. But it's a band-aid.

**Status**: In Progress (Lisa).

---

## 相关笔记

- [[Incident_Postmortem_GlobalBank_Outage]] — 同一客户 GlobalBank 的 P0 宕机事故
- [[Architecture_Defense_Python_vs_Rust]] — 此 Jira 中 VP 提议 Rust 重写，Dave 坚决反对
- [[01_PRD_Mobile_App_v2]] — GlobalBank 也是 Mobile App 的核心客户
