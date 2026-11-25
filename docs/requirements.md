# APSS Salary Calculation — Requirements & Discovery Notes

**Source:** Teams Walkthrough with Mita (Accounting) + Mita's Written Clarifications  
**Status:** Requirements confirmed and complete. Ready for architecture & implementation planning.

---

## 1. Payroll Period & Standard Working Days

- **Cut-off Period:** **26th of the previous month to 25th of the current month**  
  *(e.g., November payroll = 26 October – 25 November)*.
- **Standard Working Days:** Same calculation for all employees — total calendar days within the cut-off period **excluding Saturdays and Sundays**.

---

## 2. Employee Types & Salary Calculation Logic

### 2.1. Singapore (SG) Staff vs. Contracting Staff
- **Singapore Staff:** Salary is based on a **monthly basic salary** (not hourly calculation).
  - **CPF:** Handled separately by management (Jessie provides the CPF file at the end of each month; out of scope for system CPF calculation).
- **Contracting Staff (and Interns):** Follow the **working hours/days calculation**:
  - `Hourly rate = Basic salary / (Standard working days × 8)`  
    *(Example: `5,000,000 / (22 × 8) = 28,409.09`)*
  - Daily hour mapping:
    - **Working as normal:** `8 hours` (`100%`)
    - **Approved Annual Leave:** Already covered by basic monthly salary — **no salary deduction** (counts as `8 hours` / paid).
    - **Half-day unpaid leave:** `4 hours` (`50%`)
    - **Full-day unpaid leave / Absence:** `0 hours` (`0%`)
    - **Public Holiday (not working):** `8 hours` (`100%`)
    - **Public Holiday (working):** `16 hours` (`200%`)
- **Public Holiday Calendars:** Must be configured **per employee's local country calendar** (e.g., Vietnam, Singapore, Indonesia, Thailand, Malaysia, Myanmar).

### 2.2. Leave Management & Approval Workflow
- **Leave Submission:** Staff submit leave requests directly in the system (replacing email requests).
- **Approval & Notification Routing:**
  - Respective **Team Leader** + **Jessie** receive notifications to review and approve/reject.
  - For the **Procurement Team**, **Clea** must also be included as an approver/notified.
  - **HR** receives notifications of all leave applications and can track all requests and approval statuses.
- **Auto-Sync & Review:**
  - Approved leave records automatically reflect in attendance and payroll calculations.
  - System generates a **monthly attendance summary** for HR to review (`working days`, `approved leave`, `unpaid leave`, `absences`) before finalizing payroll.

---

## 3. Compensation Components

- **Basic Salary:** Stored in the employee's original currency (`VND`, `SGD`, `IDR`, `THB`, `MYR`, etc.), updated when incremental raises occur.
- **Commission:** Extracted from **Business Central (BC)** and calculated manually each month, then entered/imported into the system.
- **Intern Project Reward:** Additional payment component for interns, tracked separately from basic salary.

---

## 4. Currency Conversion & Payment Channels

- **Original Currency:** Transactions are recorded in their original currency in BC.
- **USD Expense Conversion:**
  - Converted to **USD** for consolidating department expenses.
  - **BC USD exchange rate** is **manually entered** into the system on the **25th** of each month (cut-off date).
- **SGD Payment Recording:**
  - Payments are recorded in **SGD** across two channels:
    - **Instarem:** Used for most staff (converted to SGD, no bank fee). **Instarem SGD exchange rate** is **manually entered after the payment has been created in Instarem**.
    - **OCBC Singapore:** Used for direct transfers to **Singapore staff** and **Myanmar staff** (since Instarem does not support Myanmar yet).
- **Bank Output Format:**
  - **Excel summary table** for checking and reviewing payroll payments is sufficient (bank upload CSV/Excel templates can be added as an enhancement if templates are provided).

---

## 5. GL Codes & Accounting Booking Output (BC)

System must generate a **payroll summary with a breakdown by GL code, department, and amount** for accounting entries in **Business Central (BC)** & checking:

| GL Code | Department / Category |
|---|---|
| `7200` | SCC (Commission) |
| `7200` | New customer |
| `7200` | e-Store |
| `8040` | Procurement |
| `8040` | Logistic |
| `8040` | BD and Marketing |
| `8040` | Account |
| `8040` | HR |
| `8040` | IT |
| `9440` | PASTOR CHE KO |

---

## 6. System Roles & Permissions

| Role | Access / Responsibilities |
|---|---|
| `Staff / Intern` | Submit leave requests, view own leave status & attendance/payslip info |
| `Approver` | Review, approve/reject leave requests for assigned team/department |
| `Payroll Viewer` | Read-only access to view payroll summaries *(assigned alongside Approver for **Jessie** and **Clea**)* |
| `HR` | Track all leave applications/statuses, review monthly attendance summary before payroll finalization |
| `Accountant (Mita)` | Manage exchange rates (BC 25th USD, Instarem SGD), commission/rewards, generate payroll, payment summaries & GL booking exports |
| `Admin` | Manage users, roles, departments, GL codes, country public holiday calendars, and system configuration |
