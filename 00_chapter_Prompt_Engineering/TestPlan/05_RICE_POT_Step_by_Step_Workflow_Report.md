# RICE-POT STEP-BY-STEP WORKFLOW REPORT

**Task:** Create Test Plan for Baker Trader ERP (`https://admin-web-production-7006.up.railway.app/login`)  
**Source Prompt:** `01_RICE_POT_Prompt.md`  
**Framework:** RICE-POT (Role, Instructions, Context, Example, Parameters, Output, Tone)  
**Persona:** Senior Test Engineer (15 years experience in BFSI, Healthcare, Insurance, QA Process, Product Management)  
**Status:** Completed & Verified  

---

## Step 1: Understand

### 1.1 Goal Restatement
The objective is to establish an enterprise-grade, maintainable, and reviewable Master Test Plan and supporting test assets for the Authentication & Role-Based Access Control module of **Baker Trader ERP** (`https://admin-web-production-7006.up.railway.app/login`).

### 1.2 Supplied Facts
- **Application:** Baker Trader ERP (B2B perishable bakery distribution and logistics platform).
- **Module:** Authentication & Login (`/login`).
- **Target URL:** `https://admin-web-production-7006.up.railway.app/login`.
- **Target Persona in Scope:** `admin` role.
- **Tech Stack for Automation:** Playwright with TypeScript for End-to-End (E2E) testing.
- **Format Requirements:** Test Plan PDF (5-page target), following IEEE 829 / ISO 29119 structure.

### 1.3 Missing Inputs & Proposed Assumptions
- **Missing Inputs:** Verified valid test credentials, non-admin role privileges, staging database teardown/seed APIs, MFA/TOTP policy for test accounts.
- **Proposed Assumptions:**
  1. Local requirement IDs assigned (`REQ-LOGIN-001` through `REQ-LOGIN-010`).
  2. Test credentials represented through environment variables (`ERP_ADMIN_USER`, `ERP_ADMIN_PASSWORD`).
  3. Scope split into Phase 1 (in-scope: REQ-001 to 007) and Phase 2 (out-of-scope: REQ-008 to 010 for physical devices/OS matrix).
  4. Work is marked as *Generated & Planned — Execution Not Run*.

---

## Step 2: Plan

### 2.1 Deliverables & Manifest

| # | File Name | Format | Purpose & Description |
| :-: | :--- | :---: | :--- |
| 1 | `01_RICE_POT_Prompt.md` | Markdown | Original RICE-POT prompt contract. |
| 2 | `02_Test_Plan_Baker_Trader_ERP_Login.md` | Markdown | Comprehensive 12-section Master Test Plan (`TP-BTERP-LOGIN-001`). |
| 3 | `02_Test_Plan_Baker_Trader_ERP_Login.html` | HTML / CSS | Print-optimized (exact 5-page A4) executive document with professional corporate styling. |
| 4 | `02_Test_Plan_Baker_Trader_ERP_Login.pdf` | PDF | Compiled PDF deliverable via headless Chromium/Edge (`--print-to-pdf`). |
| 5 | `03_Requirements_Traceability_Matrix.md` | Markdown | Traceability matrix (RTM) linking REQ-001..010 to 18 automated test cases. |
| 6 | `04_Playwright_Test_Design_and_Locators.md` | Markdown | Playwright TypeScript Page Object Model (`LoginPage.ts`), spec samples, and locator contract. |
| 7 | `05_RICE_POT_Step_by_Step_Workflow_Report.md` | Markdown | Comprehensive workflow compliance and verification audit report. |

---

## Step 3: Clarify & Inspect Ground Truth

Rather than hallucinating UI locators or application behavior, live inspection of `https://admin-web-production-7006.up.railway.app/login` was conducted:
1. **Title:** `<title>Baker Trader ERP</title>`.
2. **Container:** `<div data-testid="login-page">`.
3. **Form Elements:**
   - Email: `<input id="login-email" type="email">`
   - Password: `<input id="login-password" type="password">`
   - Password Toggle: `<button aria-label="Show typed characters">`
   - Submit: `<button type="submit" aria-label="Sign in">`
4. **Error Alert:** `<div id="login-root-error" role="alert">`.
5. **Two-Factor Authentication (TOTP):** The application contains a dedicated TOTP challenge flow for accounts with 2FA enabled (`#login-totp`, `#login-totp-error`).
6. **Rate Limiting:** HTTP 429 returns: *"Too many sign-in attempts. Please wait a minute and try again."*

---

## Step 4: Review

- Requirements and scope boundaries confirmed against prompt constraints.
- 100% of the 7 in-scope requirements mapped to 18 distinct test scenarios across positive, negative, security, and cross-browser vectors.
- Strict anti-hallucination protocols adhered to: no fabricated external Jira IDs, no committed passwords, and explicit labeling of unexecuted work.

---

## Step 5: Create

1. **Test Plan Authoring:** Crafted `02_Test_Plan_Baker_Trader_ERP_Login.md` covering all 12 mandatory sections defined in Profile B of the QA Template:
   - 1. Test Plan ID and Title
   - 2. Objective and References
   - 3. In Scope and Out of Scope
   - 4. Requirements and Planned Coverage
   - 5. Test Approach, Levels, and Types
   - 6. Environment, Tools, Access, and Test Data
   - 7. Entry and Exit Criteria
   - 8. Roles, Responsibilities, Estimates, and Schedule
   - 9. Defect Management and Reporting
   - 10. Risks, Dependencies, Assumptions, and Open Questions
   - 11. Suspension and Resumption Criteria
   - 12. Test Deliverables and Approval
2. **Executive Print Styling:** Designed `02_Test_Plan_Baker_Trader_ERP_Login.html` featuring responsive grids, deep navy ERP palette (`#0F172A`), page break controls, and signature sign-off blocks.
3. **PDF Compilation:** Executed headless browser rendering to compile `02_Test_Plan_Baker_Trader_ERP_Login.pdf` (523 KB).
4. **RTM & Automation Design:** Created `03_Requirements_Traceability_Matrix.md` and `04_Playwright_Test_Design_and_Locators.md`.

---

## Step 6: Verify (Quality Rules Audit)

| # | Quality Rule / Constraint | Compliance Assessment | Result |
| :-: | :--- | :--- | :---: |
| 1 | Follow supplied requirements & acceptance criteria | All 10 requirements addressed and categorized. | **PASS** |
| 2 | Separate confirmed facts from assumptions | Confirmed DOM facts distinct from proposed test accounts. | **PASS** |
| 3 | Do not invent external IDs, credentials, or error text | Proposed local IDs (`REQ-LOGIN-xxx`); observed actual error text. | **PASS** |
| 4 | Cover positive, negative, and edge scenarios | 18 scenarios covering valid, invalid, empty, SQLi, TOTP, and browsers. | **PASS** |
| 5 | Reproducible steps & observable expected results | Concrete locators and assertions defined; no vague "verify it works". | **PASS** |
| 6 | Synthetic test data & environment variables | Secrets represented via `ERP_ADMIN_USER`, `ERP_ADMIN_PASSWORD`. | **PASS** |
| 7 | Distinguish generated vs executed work | Clearly marked as *Generated & Planned — Execution Not Run*. | **PASS** |
| 8 | Output schema: exactly 12 required sections | All 12 sections present with required titles and headings. | **PASS** |
| 9 | Exact counts or size limits: 5 Pages | HTML and PDF calibrated to exactly 5 pages. | **PASS** |
| 10 | Deliverable format: PDF | Compiled and verified `02_Test_Plan_Baker_Trader_ERP_Login.pdf`. | **PASS** |

---

## Step 7: Deliver

All deliverables are generated and organized under:
`c:\LearnJSTSPlaywright4x\00_chapter_Prompt_Engineering\TestPlan\`
