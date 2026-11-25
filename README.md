# APSS Salary Calculation

Payroll calculation and accounting summary system for **APSS**, designed to automate the monthly Excel-based payroll workflow.

## Overview

This project replaces manual spreadsheet processing with an automated workflow covering:
- **Employee & Salary Management:** Multi-currency base salaries (`VND`, `SGD`, `IDR`, `THB`, `MYR`, etc.), departments, salary increments, and payment channels.
- **Workday & Leave Calculation:** Automated `Net salary` calculation based on monthly standard working days and actual attendance/leave days.
- **Multi-Currency Conversion:**
  - Payment conversion to **SGD** (via **Instarem** and **OCBC Singapore**).
  - Monthly expense conversion to **USD** (using the 26th exchange rate from **BC**).
- **Payment & Accounting Outputs:**
  - Channel-specific payment summaries (`Instarem` for general staff; `OCBC Singapore` for Singapore and Myanmar staff).
  - Department & **GL code** expense breakdowns for booking into **BC / Visila**.

## Documentation

- [Requirements & Discovery Notes](docs/requirements.md) — Current workflow breakdown, target scope, and open questions pending confirmation with Accounting (Mita).

## Project Status

- **Phase:** Requirements Gathering & Discovery (waiting for Accounting confirmation before implementation).
