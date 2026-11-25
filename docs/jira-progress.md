# JIRA Progress & Project Tracking

**Atlassian Project:** `APSST-16` — Salary Calculation  
**Jira Epic:** `KAN-101` — Salary Calculation and Controlled G/L Posting  
**Source Discussions:**
- Finance Discovery Call (Commission & Accounting) — 20 Nov 2026
- Accounting & Engineering Walkthrough — 25 Nov 2026
- Follow-up Written Clarifications from Accounting — 25 Nov 2026

---

## 1. Epic Goal & Architecture Context (`KAN-101`)

**Goal:** Build a controlled, secure monthly salary-calculation process in **Business Central (BC)** covering input validation, calculation, review, payment summary, and controlled G/L posting — reducing manual checking and preparing auditable accounting entries without allowing payroll to be posted silently.

### Two Distinguishable Flows
1. **Singapore Employees Flow:**
   - Currently uses an existing BC payroll function (select period, calculate, and post to G/L; period does not always move automatically and may require manual correction).
   - Paid based on monthly basic salary; CPF is handled separately by Management (CPF file provided at month-end).
2. **Overseas / Contract-Service Staff Flow:**
   - Currently booked manually, grouped by department, and converted to the required accounting currency.
   - Paid based on hourly/working-days calculation (`Basic salary / (Standard working days × 8)`).
- **Requirement:** These two flows must remain **distinguishable** in the system.

### Mandatory Controls & Release 1 Boundaries
- **Access Control:** Payroll details are restricted to authorized Payroll, Accounting, and Management users (`Payroll Viewer` for designated Management/Procurement leads). Employee submissions must **not** expose other employees' salaries.
- **Validation Gate:** Missing attendance, FX rates, department, or G/L mappings **must block the batch**.
- **Approval Gate:** Require authorized Accounting review/approval before journal creation or posting.
- **Audit Trail:** Preserve calculation inputs, approvals, generated entries, and audit history.
- **System of Record:** Business Central (BC) remains the accounting system of record.
- **Out of Scope for Release 1:**
  - Automatic bank-payment submission (Excel summary table is sufficient for R1).
  - Automatic G/L posting without authorized Accounting review.
  - Storing payroll data in broadly accessible locations.

---

## 2. JIRA Tasks Breakdown (`KAN-102` – `KAN-109`)

| Key | Task | Priority | Status | Notes / Mapping to Discovery |
|---|---|---|---|---|
| **KAN-102** | Confirm payroll rules, employee scope and security access | Medium | `To Do` *(In Progress via Discovery)* | **Description:** Confirm approved employee groups, monthly cut-off, working-day rules, leave/half-day treatment, commission rules, FX source, rounding, payment channels, G/L accounts, approvers, and payroll-data access. Document decisions before development starts.<br>**Status Note:** Accounting has answered most business rules (see `docs/requirements.md`). Only rounding rules, Finance sign-off on G/L codes, and R1 BC posting mode remain. |
| **KAN-103** | Design monthly salary input and payroll data model | Medium | `To Do` | **Description:** Define payroll setup, monthly header and employee salary lines, attendance inputs, department allocation, commission, currency, exchange rate, payment channel, G/L mapping, status, and audit fields. Keep salary data access restricted. |
| **KAN-104** | Build attendance, leave and working-day calculation | Medium | `To Do` | Includes leave submission, Team Leader + Management Approver (+ Procurement Approver for Procurement) notifications & approval, HR tracking, country-specific Public Holidays (8h/16h), and monthly attendance summary. |
| **KAN-105** | Build posting preview, validation and approval flow | Medium | `To Do` | Batch validation (block on missing data), preview of proposed accounting entries, and Accounting review/approval flow. |
| **KAN-106** | Test reconciliation, permissions, audit trail and month-end process | Medium | `To Do` | Verify SGD payment vs USD expense reconciliation, role-based access (`Payroll Viewer`, `Approver`, `HR`, `Accountant`, `Admin`), audit logs, and month-end close. |
| **KAN-107** | Configure department and G/L account mappings | Medium | `To Do` | Map departments to Finance-verified G/L codes (`7200`, `8040`, `9440`, etc.). |
| **KAN-108** | Build posting preview, validation and approval flow | Medium | `To Do` | *(Note: Duplicate summary with KAN-105 in Jira — clarify/merge or split between SG vs Overseas flow).* |
| **KAN-109** | Create controlled G/L journal and posting integration in BC | Medium | `To Do` | Integrate approved payroll batch into Business Central G/L journal (controlled creation/posting). |

---

## 3. Open Decisions Status (JIRA vs. Accounting Responses)

| Open Decision (from JIRA) | Status | Resolution / Next Action |
|---|---|---|
| Exact salary formula and divisor rules | **Mostly Resolved** | SG = monthly basic salary; Contracting = `Basic / (Standard working days × 8)`. **Need to confirm:** exact rounding rules per currency. |
| Attendance cut-off and weekends / public holidays | **Resolved** | Cut-off: 26th prev month – 25th current month; exclude Sat & Sun; Public Holidays per employee's local country (8h off / 16h worked). |
| Paid leave, unpaid leave, overtime and half-day rules | **Resolved** | Approved annual leave = paid (no deduction); unpaid leave = 0h full day / 4h half day. *(Confirm if any regular OT besides Public Holiday exists).* |
| Commission calculation and posting treatment | **Resolved** | Extracted from BC & calculated manually each month (manual entry/import); Interns also have separate Project Reward; G/L `7200` (SCC). |
| Approved exchange-rate source, date and rounding | **Mostly Resolved** | USD from BC as of 25th (manual input); SGD from Instarem after payment creation (manual input). **Need to confirm:** FX rounding precision. |
| Final department and G/L mappings | **Pending Finance Sign-off** | Proposed mapping: `7200` (SCC, New customer, e-Store), `8040` (Procurement, Logistic, BD & Marketing, Account, HR, IT), `9440` (PASTOR CHE KO). *Finance must formally verify G/L numbers before production posting.* |
| Approval roles & R1 journal-only vs controlled posting | **Partially Resolved** | Leave approvers & Payroll Viewer roles confirmed. **Need to confirm:** Does R1 create an unposted G/L journal in BC only, or support controlled posting? |
| Required payment export or external payment integration | **Resolved** | No automatic bank submission in R1; Excel summary table for checking/reviewing payments is sufficient. |
| **Architecture Question (New)** | **Needs Alignment** | Since staff/interns need to submit leave & view status without exposing salaries and without requiring BC licenses for everyone, confirm whether R1 uses a **separate web portal + BC integration** or **BC-only + external form**. |
