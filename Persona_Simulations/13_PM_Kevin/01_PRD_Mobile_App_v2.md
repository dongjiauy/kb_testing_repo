# Product Requirements Document (PRD): Mobile App v2.0
**Status**: Delayed
**Owner**: Kevin

## 1. Objective
Launch a native iOS/Android app to replace the laggy web wrapper.
Goal: Improve App Store rating from 2.5 to 4.5.

## 2. Core Features
- **Offline Mode**: Allow users to view downloaded reports without signal (Critical for Sales users).
- **Push Notifications**: Real-time alerts for @mentions.
- **Biometric Login**: FaceID/TouchID.

## 3. The "Blocker"
- **Backend API**: The new Rust API is not ready. We are mocking data.
- **Design**: The "Dark Mode" UI tested poorly with older users (GlobalBank execs hate it).

## 4. Revised Timeline
- Alpha: April 15 (was March 1).
- Beta: May 1.
- Launch: June 1.

---

## 相关笔记

- [[RFC_Vector_Search_Optimization]] — PRD 中提到 Rust API 未就绪，对应的技术背景
- [[03_User_Research_Interview_Notes]] — PRD 背后的用户调研依据
- [[04_Excel_Data_Export_DAU]] — PRD 目标对应的当前 DAU 指标
- [[05_Jira_Ticket_Export_Bug]] — GlobalBank 客户的相关 P0 bug
- [[Product_PRD_Knowledge_Base_v2_FINAL]] — 同公司的另一个产品 PRD
