# Code Review (GitHub PR #142)
**Author**: Ben (Junior)
**Reviewer**: Dave (Lead)
**File**: backend/api/auth.py

## Comment
Ben, this is a security risk. You are storing the JWT (JSON Web Token) in LocalStorage. This makes us vulnerable to XSS attacks.

> `localStorage.setItem('token', response.token);`

## Fix Required
- Store it in a HttpOnly Cookie.
- Ensure SameSite=Strict.
- This prevents JavaScript from reading the token.

## Also
- Remove the console.log() statements on line 45. We don't want to log user IDs in production.

**Status**: Changes Requested.
**Deadline**: Before 5 PM today. GlobalBank security audit starts tomorrow.

---

## 相关笔记

- [[Incident_Postmortem_GlobalBank_Outage]] — GlobalBank 安全审计压力下发生的 P0 事故
- [[01_PRD_Mobile_App_v2]] — 移动端 App 同样涉及登录安全需求
