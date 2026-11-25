# APSS Salary Calculation

Payroll calculation and accounting summary system for **APSS**, designed to automate and control the monthly payroll workflow and Business Central (BC) G/L integration.

## Overview

This project replaces manual spreadsheet processing with a controlled, auditable workflow covering:
- **Employee & Salary Management:** Multi-currency base salaries (`VND`, `SGD`, `IDR`, `THB`, `MYR`, etc.), departments, salary increments, and payment channels.
- **Leave Management & Workday Calculation:** Built-in leave requests, approval routing, country-specific Public Holiday calendars, and automated `Net salary` calculation based on monthly standard working days and actual attendance.
- **Two Distinguishable Payroll Flows:**
  - **Singapore Staff:** Monthly basic salary flow aligned with Business Central payroll (CPF handled separately by Management).
  - **Overseas / Contracting Staff:** Working-hours/days calculation (`Basic salary / (Standard working days × 8)`), department allocation, and multi-currency conversion.
- **Multi-Currency Conversion:**
  - Payment conversion to **SGD** (via **Instarem** and **OCBC Singapore**).
  - Monthly expense conversion to **USD** (using the 25th cut-off exchange rate from **Business Central**).
- **Payment & Accounting Outputs:**
  - Channel-specific payment summaries (`Instarem` for general staff; `OCBC Singapore` for Singapore and Myanmar staff).
  - Department & **G/L account** expense breakdowns with validation gates and Accounting review before posting to **Business Central (BC)**.

## Documentation

- **Requirements & Business Rules:** [English](docs/requirements.md) | [Tiếng Việt](docs/requirements.vi.md)
- **JIRA Progress & Delivery Tracking (`KAN-101`):** [English](docs/jira-progress.md) | [Tiếng Việt](docs/jira-progress.vi.md)

## Project Status

- **Jira Epic:** `KAN-101` (`APSST-16` — Salary Calculation)
- **Phase:** Requirements Confirmation (`KAN-102`) & Architecture / Data Model Design (`KAN-103`)
