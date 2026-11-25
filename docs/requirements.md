# APSS Salary Calculation — Requirements & Discovery Notes

**Source:** Teams Walkthrough with Mita (Accounting) on `Copy of 2020-10 Payroll summary - Example`  
**Status:** Waiting for Mita's confirmation & clarification on open questions.

---

## 1. Current Workflow (As-Is)

1. **Monthly Data Collection (24th–25th of each month):**
   - Mita emails the payroll report template to staff/departments to fill in and return.
   - Upon receiving the forms, Mita cross-checks them against her separate **attendance/leave file**.
   - **Common manual error:** Some submitters calculate using the wrong number of standard working days in the month (e.g., dividing by 22 days when the month has 23 working days).
   - **Commission:** Calculated separately by **Nga**.

2. **Payment Channels:**
   - **Instarem** (`Instarem Summary` tab): Used for most staff (converted to **SGD**, no bank transfer fee). Amounts are aggregated per person and funded from the company's main bank account.
   - **OCBC Singapore** (`Sum SG staff` tab): Used for direct transfers to **Singapore staff** and **Myanmar staff** (Instarem does not support transfers to Myanmar yet; Myanmar payments are handled/reimbursed separately).

3. **Expense Summary & Currency Conversion (`Sum contracting Srv` & `Summary` tabs):**
   - Tracks `Basic salary` (updated for incremental raises), manual `#Workdays` (e.g., `20` for full month, `19.5` for half-day leave), and `Net salary` in original currency (`VND`, `IDR`, `THB`, `MYR`, `SGD`, etc.).
   - Converts monthly expenses to **USD** using the **exchange rate on the 26th from BC** (Business Central), alongside the SGD payable amount from Instarem.
   - Summarizes expenses by **department** (`Account`, `IT`, `Logistics`, `Other Operation`, `Procurement`, `BD and Marketing`, `Pastor Che Ko / Casa Chico`) and **payment channel** (`Instarem` vs `OCBC Singapore`).

4. **Accounting Booking (BC / Visila):**
   - Allocates expenses by **GL code** at the bottom of `Sum contracting Srv`:
     - `Commission` → GL `7200` (SEC)
     - `Procurement` → GL `84040`
     - `Pastor Che Ko` (Casa Chico) → GL `9440`
     - *(Other department GL codes to be confirmed)*

---

## 2. Target System Goals (To-Be)

- Manage employee profiles, departments, base salaries in multiple currencies, salary increments, and assigned payment channels (`Instarem` / `OCBC`).
- Automate `Net salary` calculation based on standard working days per month and actual working/leave days to eliminate formula errors.
- Handle multi-currency conversions:
  - Original currency → **SGD** (for Instarem / OCBC payments)
  - Original currency → **USD** (using BC 26th exchange rate for monthly expense reporting)
- Generate payment summaries for **Instarem** and **OCBC** and export **department / GL code** breakdowns for booking into **BC / Visila**.

---

## 3. Open Questions (Sent to Mita — Pending Reply)

1. **Monthly Data Input:**
   - *Question:* Will the new system replace the emailed forms (staff/departments input directly or pull from attendance), or will Mita still import the attendance file?
   - *Mita's Answer:* *(Pending)*

2. **Standard Working Days & Leave Calculation:**
   - *Question:* How are standard working days per month (20, 22, 23...) determined (same for all or per country/group)? What are the exact formulas for `Deduction` (SG staff) vs `#Workdays` (contracting staff)?
   - *Mita's Answer:* *(Pending)*

3. **Commission & Other Allowances/Deductions:**
   - *Question:* Will Nga's Commission figures be manually entered/imported, or calculated by the system? Are there any other allowances, bonuses, or deductions?
   - *Mita's Answer:* *(Pending)*

4. **Exchange Rates & Bank Upload Formats:**
   - *Question:* Will Mita manually input the 26th BC exchange rates and Instarem rates each month? Do Instarem and OCBC require specific export file templates/formats for bank upload?
   - *Mita's Answer:* *(Pending)*

5. **Full GL Code List & Booking Format:**
   - *Question:* What is the complete list of GL codes for all remaining departments, and what exact report/file format is needed for booking into BC / Visila?
   - *Mita's Answer:* *(Pending)*
