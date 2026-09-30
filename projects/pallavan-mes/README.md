<div align="center">

# Pallavan MES

**An experimental Offline-First Manufacturing Execution System (MES).**

A rigorous technical exploration of factory-floor software, focusing on offline-first sync engines, IndexedDB, and Role-Based Access Control in low-connectivity environments.

<p>
  <img src="https://img.shields.io/badge/Status-Completed-success?style=flat-square" />
  <img src="https://img.shields.io/badge/Access-Private-red?style=flat-square" />
</p>

[View Live Demo](https://pallavan-mes.web.app)

</div>

---

## Introduction

This project is an experimental Manufacturing Execution System (MES) built specifically to explore how software can function on a factory floor. The core challenge in industrial software is the "Offline Factory" problem: environments with highly unreliable internet connections where data loss is unacceptable.

Designed entirely as an experiment to understand MES architecture, this application enables operators, supervisors, and managers to track production output, monitor machine efficiency, and ensure quality control—even when the network completely fails. 

> **Disclaimer & Security Note:** This is an experimental proof-of-concept project intended for architectural exploration. Because it was built to test offline sync mechanics and role-based UI flows without heavy backend enforcement, it contains deliberate security vulnerabilities (e.g., using universal demo credentials and relying primarily on client-side logic). It is not intended for true production use without a comprehensive security overhaul.

## The Problem

Factory floor environments require extreme predictability and speed. If the network drops, operators cannot stop working to wait for a loading spinner. If multiple operators are entering data, the system needs to resolve conflicts and sync states seamlessly once the connection is restored. Standard web applications fail under these conditions.

## What I Built

I built a complete, offline-first MES utilizing React 19, TypeScript, and Dexie.js. It encompasses a full workflow: creating forms, real-time metrics calculation, hierarchical role-based access control (RBAC), and PDF/Excel export capabilities. 

To ensure factory floor predictability, the application is strictly gated against mobile devices, enforcing access only via controlled Desktop Kiosks.

## Key Features

- **True 2-Way Offline Sync Engine:** The app functions 100% offline using IndexedDB as the primary source of truth. A background worker pushes local writes to Firebase, while an active `onSnapshot` listener pulls down remote updates. 
- **Lightning Fast Reactivity:** By utilizing `dexie-react-hooks` (`useLiveQuery`), the React UI updates instantly from local storage without waiting for network roundtrips.
- **Hierarchical RBAC Privacy:** Complete data separation. Operators (can save private "Drafts"), Supervisors (can review and approve submissions), and Managers (exclusive access to Analytics and Exports).
- **Graceful Error Trapping:** A global Error Boundary prevents fatal crashes from data mutations, displaying a recovery UI rather than the dreaded "White Screen of Death."
- **PWA Service Worker Integration:** Aggressive caching ensures that if the internet dies and a user accidentally refreshes, the app reloads instantly instead of failing.
- **Summary Analytics:** Management-exclusive Recharts-based data visualizations breaking down rejection rates by machine and Pareto charts for rejection reasons.

## Technical Architecture

The application is built with a rigorous Offline-First philosophy:

- **Local Database:** Dexie.js (IndexedDB wrapper) acts as the absolute source of truth for the UI.
- **Cloud Database:** Firebase Firestore handles long-term storage and cross-device syncing.
- **Sync Worker:** Continuously monitors IndexedDB for pending entries and pushes them to Firebase when online. If offline, data sits safely on the device.

## Tech Stack

<p>
  <img src="https://img.shields.io/badge/React-20232A?style=flat-square&logo=react&logoColor=61DAFB" />
  <img src="https://img.shields.io/badge/TypeScript-007ACC?style=flat-square&logo=typescript&logoColor=white" />
  <img src="https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=vite&logoColor=white" />
  <img src="https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=flat-square&logo=tailwind-css&logoColor=white" />
  <img src="https://img.shields.io/badge/Firebase-FFCA28?style=flat-square&logo=firebase&logoColor=black" />
  <img src="https://img.shields.io/badge/IndexedDB-000000?style=flat-square" />
</p>

## Engineering Highlights

### Mathematical Constraints & Auto-Calculations
The initial concept didn't strictly prevent impossible physical states. I added hard mathematical validations preventing operators from rejecting more parts than were produced and implemented auto-calculating "Total Quantity" fields to dramatically reduce manual entry error.

### Draft Lockout Prevention
A known edge case with Draft saving is that an operator could create a Draft for a specific Shift/Machine/Time, essentially locking that time slot, but never submit it. I modified the global deduplication engine to explicitly ignore Drafts, allowing a secondary operator to take over that time slot if the original operator abandons their Draft.

### Dirty Data Retention Prevention
If an operator enters 30 mins downtime, selects "Belt Snap", and later corrects downtime to 0, leaving "Belt Snap" in the database pollutes analytics. I implemented an auto-clear effect for conditional reasons when their trigger values hit zero.

### Shift C Midnight Rollover
Shift C runs from 22:00 to 06:00. To prevent date-splitting confusion in exports, I designated `entryDate` as the unified "Shift Date", and explicitly labeled the midnight-crossing slots with `(+1d)` in the UI.

## Honest Assessment & Future Improvements

**Strengths:** The domain modeling and offline sync architecture are robust. By enforcing IndexedDB as the singular source of truth for the React UI, the application achieves zero-latency renders and is completely immune to network dropouts. The code is modular, aggressively type-safe, and highly polished.

**Weaknesses:** The 2-way sync engine currently operates on a full-collection sync mechanism. While perfectly performant for thousands of entries, as the database grows to hundreds of thousands of historical entries, the initial Dexie population load would require pagination and targeted queries rather than a bulk snapshot sync.

**If I continued development:**
- **Hardware API Integration:** In a true factory setting, manually typing batch numbers is prone to error. I would integrate the Web Serial API or generic Barcode Scanner listeners to automatically populate the Batch Number and Machine ID fields.
- **Conflict Resolution UI:** Currently, the 2-way sync engine uses a "last write wins" protocol via Firebase Timestamps. In a highly distributed environment, I would build a conflict resolution modal that allows Supervisors to manually resolve state collisions if two operators edit the same entry offline simultaneously.

## Getting Started (Demo)

Because this is an experimental project, you can access the live demo and use the following universal credentials to explore the different RBAC environments:

- **Universal PIN:** `apex123`
- **Operator IDs:** `OP-01`, `OP-02`
- **Supervisor ID:** `SUP-01`
- **Manager ID:** `MGR-01`
