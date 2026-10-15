# TEST PLAN: BAKER TRADER ERP — AUTHENTICATION & LOGIN MODULE

**Document ID:** TP-BTERP-LOGIN-001  
**Version:** 1.0.0 (Proposed / Under Review)  
**Author:** Senior Test Engineer (QA Process, Product Management, Team Building)  
**Application Under Test (AUT):** Baker Trader ERP  
**Module:** Authentication & Login (`/login`)  
**Target URL:** https://admin-web-production-7006.up.railway.app/login  
**Domain:** Enterprise Resource Planning (ERP — B2B Bakery Distribution & Logistics)  
**Execution Status:** Generated & Planned — Execution Not Run  

---

## 1. Test Plan ID and Title

- **Test Plan Identifier:** `TP-BTERP-LOGIN-001`
- **Document Title:** Master Quality Assurance Test Plan for Baker Trader ERP Authentication & Role-Based Access Control Module
- **Classification:** Internal Engineering & QA Deliverable
- **Revision History:**

| Version | Date | Author / Role | Summary of Changes | Approval Status |
| :--- | :--- | :--- | :--- | :--- |
| `0.1.0` | 2026-10-15 | Senior Test Engineer | Initial draft generated following RICE-POT prompt parameters. | Pending Review |
| `1.0.0` | 2026-10-15 | Senior QA Architect | Complete 12-section test plan with Playwright TS E2E automation scope, RTM, and defect governance. | Proposed for Baseline |

---

## 2. Objective and References

### 2.1 Business & Quality Objectives
1. **Security & Access Assurance:** Guarantee that only authorized business personas (starting with the `admin` role) can access confidential B2B distribution records, inventory ledger, vendor procurement data, and delivery notes.
2. **Defect Prevention & Boundary Validation:** Detect vulnerabilities, input validation gaps (e.g., special characters, empty fields, SQLi/XSS probes), and rate-limiting failures before production rollout.
3. **Cross-Browser & Cross-Device Compatibility:** Ensure deterministic rendering, responsive layouts, and reliable user interactions across modern Chromium, WebKit, and Gecko desktop browsers.
4. **Automated Regression Readiness:** Establish a robust Playwright + TypeScript test harness with Page Object Models (POM) to integrate into CI/CD pipelines.

### 2.2 Reference Documents & Standards
- **Supplied Requirement Prompt:** `01_RICE_POT_Prompt.md` (instantiated from RICE-POT generic framework).
- **Target Application Environment:** https://admin-web-production-7006.up.railway.app/login
- **Industry Standards:** IEEE 829-2008 (Standard for Software and System Test Documentation), ISO/IEC/IEEE 29119-3 (Software Testing).
- **Security Guidance:** OWASP ASVS v4.0.3 (Authentication Verification Standard), OWASP Top 10 (A07:2021 Identification and Authentication Failures).

---

## 3. In Scope and Out of Scope

### 3.1 In Scope (Phase 1 — Playwright E2E & Functional Testing)
The following features and verification points are confirmed in scope per project parameters:

1. **Page Availability & Metadata (REQ-LOGIN-001):** Verification of HTTP 200/loaded state, DOM readiness, page title (`Baker Trader ERP`), favicon, and canonical route `/login`.
2. **Valid Authentication Flow (REQ-LOGIN-002):** Successful login with valid `admin` credentials, token persistence (`access_token` in local state/bearer headers), and deterministic redirection to dashboard (`/` or `/today`).
3. **Invalid Credentials Handling (REQ-LOGIN-003):** Rejection of incorrect email/password combinations with user-friendly alert message (`Email or password is incorrect.`) without system stack exposure.
4. **Empty Credentials Validation (REQ-LOGIN-004):** Client-side validation preventing blank submissions (`required` attributes, inline error badges).
5. **Special Characters & Boundary Inputs (REQ-LOGIN-005):** Handling of unicode characters, escaped quotes, SQL syntax fragments, and extended characters in email and password fields without application crashes.
6. **Role-Based Access Control (RBAC) Verification (REQ-LOGIN-006):** Validating role separation (`admin` vs planned roles such as warehouse manager, route driver, sales agent).
7. **Cross-Browser Verification (REQ-LOGIN-007):** Execution across desktop Chromium (Google Chrome / Microsoft Edge), Firefox (Gecko), and WebKit (Safari engine) via Playwright test runners.

### 3.2 Out of Scope (Deferred to Subsequent Phases)
The following items were identified in generic requirements but are explicitly marked Out of Scope for Phase 1 Playwright TS automation:

1. **Native Mobile OS Testing (REQ-LOGIN-008):** Native Android/iOS mobile application packaging (scope is strictly responsive web portal).
2. **Physical Hardware Device Matrix (REQ-LOGIN-009):** Device-level lab testing on legacy physical handsets (deferred to mobile farm testing in Phase 2).
3. **Multi-Resolution Responsiveness Below 360px (REQ-LOGIN-010):** Ultra-wide 4K/8K or screens below 360px width; desktop (1280x720, 1920x1080) and tablet viewports are standard.
4. **Backend Database Direct Mutation / Pen-Testing:** Destructive penetration testing, DDoS simulation, or direct database tampering without authorization.
5. **SSO / Third-Party SAML/OAuth Integration:** Social login or federated identity (not supported by current Baker Trader ERP architecture).

---

## 4. Requirements and Planned Coverage

All requirements are assigned local traceability IDs (`REQ-LOGIN-xxx`) to ensure auditability.

| Local Requirement ID | Requirement Description | Test Scenarios Planned | Priority | Planned Coverage |
| :--- | :--- | :--- | :--- | :--- |
| `REQ-LOGIN-001` | Login Page Should be loaded Properly with title and URL. | DOM element presence, title assertion, favicon verification, secure HTTPS protocol validation. | P1 (Critical) | 100% |
| `REQ-LOGIN-002` | Verify user can login with valid credentials. | Valid admin authentication, bearer token receipt, redirect to landing dashboard, state preservation. | P1 (Critical) | 100% |
| `REQ-LOGIN-003` | Verify user cannot login with invalid credentials. | Incorrect password, non-existent email, invalid domain format; verification of role alert `#login-root-error`. | P1 (Critical) | 100% |
| `REQ-LOGIN-004` | Verify validation on empty credentials. | Blank email, blank password, whitespace-only strings; verification of form submission suppression and HTML5/custom validation. | P2 (High) | 100% |
| `REQ-LOGIN-005` | Verify credentials containing special characters. | Input containing quotes (`'`, `"`), HTML tags (`<script>`), SQL injection patterns (`' OR 1=1 --`), and extended unicode/accents. | P2 (High) | 100% |
| `REQ-LOGIN-006` | Verify login with different business roles. | Admin role access verification, permission boundary checks for downstream routes (`/admin/users`, `/masters`, `/reports`). | P2 (High) | 100% (Admin verified; non-admin roles mocked/proposed) |
| `REQ-LOGIN-007` | Verify login across different desktop browsers. | Playwright multi-project execution across Chromium, Firefox, and WebKit on Windows and Linux runners. | P2 (High) | 100% (Desktop matrix) |
| `REQ-LOGIN-008` | Verify login across different operating systems. | Proposed for Phase 2; Playwright CI matrix will run on Ubuntu and Windows. | P3 (Medium) | Out of Phase 1 Scope |
| `REQ-LOGIN-009` | Verify login on different physical devices. | Proposed for Phase 2 mobile emulator test cycle. | P3 (Medium) | Out of Phase 1 Scope |
| `REQ-LOGIN-010` | Verify login across multiple screen resolutions. | Responsive viewport validation at 1920x1080, 1366x768, and 768x1024 tablet breakpoint. | P3 (Medium) | Deferred to UI regression |

---

## 5. Test Approach, Levels, and Types

### 5.1 Test Levels
1. **Component / Integration Level:**
   - Client-side validation behavior in React (`react-hook-form` validation schemas).
   - API contract testing for `/api/auth/login` (request payload, status codes: 200, 401, 403, 429).
2. **System / End-to-End (E2E) Level:**
   - Complete browser-driven workflows using Playwright with TypeScript.
   - Page Object Model (POM) architecture isolating page locators and interaction flows from assertion logic.
3. **Regression Level:**
   - Automated regression suite executed on pull requests and nightly builds.

### 5.2 Applicable Test Types
- **Functional Testing:** Form submission, validation messaging, credential verification, state navigation.
- **Negative & Edge Testing:** Malformed emails, excessive length inputs (>255 chars), rapid consecutive submissions, session expiry.
- **Security-Focused Testing:** Masking of password characters, toggle visibility button (`Hide/Show typed characters`), avoidance of password exposure in console/network logs, rate-limiting behavior (HTTP 429).
- **Two-Factor Authentication (TOTP) Workflow Testing:** Detection of `totp_required` challenge state and transition to 6-digit TOTP verification view.
- **Cross-Browser Compatibility:** Multi-browser test execution in Playwright headless and headed modes.

---

## 6. Environment, Tools, Access, and Test Data

### 6.1 Target Environments
- **Primary Test Environment (Staging/Production Mirror):**  
  `https://admin-web-production-7006.up.railway.app/login`
- **Hosting Infrastructure:** Railway Containerized Cloud Service
- **Backend Auth Endpoint:** `/api/auth/login` and `/api/auth/login/totp`

### 6.2 Test Automation Tooling & Technology Stack

| Category | Tool / Framework | Version / Configuration | Purpose |
| :--- | :--- | :--- | :--- |
| **Language** | TypeScript | `^5.x` | Strongly-typed test scripting |
| **Runtime** | Node.js | `v26.x` / LTS `v20.x` | Execution runtime |
| **E2E Test Runner** | Playwright Test | `^1.45.0` or latest stable | Browser automation & assertion engine |
| **Design Pattern** | Page Object Model (POM) | Custom `LoginPage.ts` class | Maintainability and locator encapsulation |
| **Assertion Library** | `@playwright/test` `expect` | Web-first assertions with auto-retries | Stable assertion and timeout handling |
| **Reporting** | Playwright HTML Reporter / Allure | Built-in + JUnit XML | Stakeholder and CI/CD test reports |
| **CI/CD Platform** | GitHub Actions / Azure DevOps | Proposed standard workflow | Automated test gate on Pull Requests |

### 6.3 Test Data Strategy & Secrets Management
> [!IMPORTANT]
> **Zero Hardcoded Secrets Policy:** No passwords, bearer tokens, or sensitive user accounts shall be committed to version control.

- **Environment Variables:** Credentials must be supplied via `.env` file (local development) or GitHub Action Secrets (CI/CD):
  - `ERP_BASE_URL=https://admin-web-production-7006.up.railway.app`
  - `ERP_ADMIN_USER=admin@bakertrader.internal` (Synthetic proposed credential)
  - `ERP_ADMIN_PASSWORD=*******` (Injected securely at runtime)
- **Synthetic Test Data Sets:**
  - `Valid Credentials:` Dedicated automated test service account with `admin` privileges.
  - `Invalid Credentials:` `invalid.operator@example.invalid` / `WrongPassword!99`
  - `Malformed Email Data:` `missing-at-sign.com`, `user@.com`, `plainword`
  - `Special Characters Data:` `admin+test'quote"@bakertrader.com`, `<script>alert(1)</script>`, `SELECT * FROM users;`
  - `TOTP Data:` Synthetic 6-digit mock code (`123456`) or RFC 6238 time-based authenticator seed.

---

## 7. Entry and Exit Criteria

### 7.1 Entry Criteria (Phase Execution Preconditions)
Execution of this test plan may commence only when the following criteria are fulfilled:
1. Target test environment URL is accessible (`HTTP 200` on `/login`).
2. Test automation repository initialized with Playwright, TypeScript, and required dependencies.
3. Test account credentials provisioned and verified for active status.
4. Test Plan `TP-BTERP-LOGIN-001` reviewed and approved by QA Lead and Product Owner.
5. CI/CD test agent has network connectivity to Railway endpoints.

### 7.2 Exit Criteria (Phase Completion & Release Gates)
Testing is formally concluded when all of the following conditions are satisfied:
1. **Planned Test Coverage:** 100% of agreed In-Scope test scenarios (REQ-LOGIN-001 through REQ-LOGIN-007) are authored, reviewed, and executed.
2. **Pass Rate Threshold:** $\ge 98\%$ automated test pass rate across supported desktop browsers.
3. **Defect Status:**
   - **Critical (P1 / Severity 1):** 0 open defects.
   - **High (P2 / Severity 2):** 0 open defects (or explicit documented sign-off from Product Owner).
   - **Medium / Low (P3/P4):** Remediation plan and target release documented in Jira.
4. **Test Summary Deliverable:** Final Automated Test Execution Report generated and archived.

---

## 8. Roles, Responsibilities, Estimates, and Schedule

### 8.1 RACI Responsibility Matrix

| Team Member / Role | Responsible (R) | Accountable (A) | Consulted (C) | Informed (I) |
| :--- | :---: | :---: | :---: | :---: |
| **Senior Test Engineer** | Test Plan Authoring, Playwright POM & Test Scripting | Test Suite Quality | Dev Lead | Product Owner |
| **QA Automation Lead** | Framework Setup, CI/CD Pipeline integration | Sign-off & Quality Gates | DevOps Lead | Engineering Manager |
| **Backend / Fullstack Developer** | Bug Fixes, Test Account Provisioning | Auth API Stability | Senior Test Engineer | QA Lead |
| **Product Owner** | Requirement Sign-off, Acceptance Approval | Release Decision | QA Lead | Executive Stakeholders |

### 8.2 Workload Estimates & Schedule

| Phase / Activity | Task Breakdown | Estimated Effort | Target Milestone |
| :--- | :--- | :---: | :--- |
| **Phase 1: Planning** | Test Plan & RTM authoring, locator inspection | 1 Day | Sprint Day 1 |
| **Phase 2: Framework & POM** | BasePage, LoginPage POM, Playwright configuration | 1.5 Days | Sprint Day 2–3 |
| **Phase 3: Test Implementation** | Scripting 18 automated test cases (REQ-001 to 007) | 2.5 Days | Sprint Day 4–6 |
| **Phase 4: CI/CD & Cross-Browser** | GitHub Actions matrix, multi-browser run, reporting | 1 Day | Sprint Day 7 |
| **Phase 5: Review & Reporting** | Execution analysis, defect triage, sign-off report | 1 Day | Sprint Day 8 |
| **Total Estimated Effort** | | **7 Person-Days** | |

---

## 9. Defect Management and Reporting

### 9.1 Defect Classification Matrix

| Severity Level | Definition | SLA / Resolution Time | Example in Login Scope |
| :--- | :--- | :--- | :--- |
| **Critical (Sev 1)** | Complete blocker. Application crash, total inability to log in, unhandled security vulnerability. | Within 4 hours | Valid credentials result in blank page or 500 error; password revealed in plain text URL. |
| **Major (Sev 2)** | Major feature broken; no viable workaround available. | Within 24 hours | Invalid credentials allow unauthorized entry; submit button unresponsive on WebKit. |
| **Moderate (Sev 3)** | Feature impairment with acceptable workaround. | Within 3 business days | Error message text grammatically incorrect or misaligned; focus not set on email field. |
| **Minor (Sev 4)** | Cosmetic flaw, minor styling or UI inconsistency. | Backlog prioritization | Inconsistent padding on staff portal footer text on 1366px screen. |

### 9.2 Defect Triage & Reporting Workflow
1. **Logging:** Log all failed checks in Jira under component `AUTH-LOGIN` with reproduction steps, environment URL, browser/OS version, Playwright trace files (`trace.zip`), and failure screenshots.
2. **Daily Standup & Triage:** Daily 15-minute QA-Dev sync to review incoming defects and assign priority.
3. **Retesting:** Automated re-execution via Playwright tag `@retest` once fix is deployed to staging.

---

## 10. Risks, Dependencies, Assumptions, and Open Questions

### 10.1 Identified Business & Technical Risks

| Risk ID | Description | Impact | Probability | Mitigation Strategy |
| :--- | :--- | :---: | :---: | :--- |
| **RSK-01** | **Rate Limiting (HTTP 429):** Rapid automated parallel execution may trigger rate limits and fail valid tests. | High | High | Implement exponential backoff, worker throttling (`workers: 2`), or dedicated whitelist for test IPs. |
| **RSK-02** | **Two-Factor Authentication (TOTP):** Admin accounts requiring TOTP code will block headless automated tests without dynamic token generation. | High | Medium | Provision a dedicated non-TOTP test account or integrate `otplib` into Playwright fixtures to compute 6-digit TOTP codes dynamically. |
| **RSK-03** | **Environment Instability on Railway:** Cloud staging environment may experience spin-down or cold starts. | Medium | Medium | Implement health-check pre-test ping to `/login` to warm up container before test execution. |

### 10.2 Confirmed Facts vs. Proposed Assumptions

| Category | Item Description | Status |
| :--- | :--- | :--- |
| **Confirmed Fact** | Target application is a React Single Page Application hosted on Railway (`/login`). | Verified via DOM/bundle inspection |
| **Confirmed Fact** | Email input (`id="login-email"`), Password input (`id="login-password"`), and Submit button exist. | Verified in DOM structure |
| **Confirmed Fact** | Error alert container utilizes `#login-root-error` with `role="alert"`. | Verified in DOM structure |
| **Proposed Assumption** | Test execution assumes an active `admin` account will be provisioned by backend team. | Pending confirmation |
| **Proposed Assumption** | Cross-browser scope covers standard desktop resolutions (1280x720, 1920x1080). | Proposed standard |
| **Open Question** | Are non-admin roles (e.g., Driver, Warehouse Manager, Sales) equipped with portal login rights or separate apps? | To be clarified with Product Owner |

---

## 11. Suspension and Resumption Criteria

### 11.1 Suspension Criteria
Test execution shall be immediately suspended under any of the following conditions:
1. Target login URL returns `502 Bad Gateway`, `503 Service Unavailable`, or fails to load for >10 consecutive minutes.
2. An open Critical (Sev 1) defect blocks the primary authentication flow across all browsers.
3. Test credentials are invalidated or revoked with no immediate replacement.
4. Staging environment undergoes unannounced schema migrations or downtime.

### 11.2 Resumption Criteria
Testing shall resume when:
1. The blocking defect or environment failure is resolved and confirmed via automated health check.
2. The QA Lead verifies deployment of the hotfix to the staging environment.
3. Test accounts and database state are restored to baseline.

---

## 12. Test Deliverables and Approval

### 12.1 Planned Deliverables
1. **Master Test Plan:** `TP-BTERP-LOGIN-001` (Markdown and PDF formats).
2. **Requirements Traceability Matrix (RTM):** Documenting requirement-to-test mapping.
3. **Playwright Automation Test Harness:** Complete TypeScript test repository including `LoginPage` POM and test specifications.
4. **Automated Test Run Reports:** Interactive HTML report with traces, videos, and screenshots for failed tests.
5. **Final Test Summary Report:** Release readiness assessment document.

### 12.2 Formal Sign-off & Approvals

| Stakeholder Role | Name / Title | Signature | Decision (Approved / Rejected) | Date |
| :--- | :--- | :--- | :--- | :--- |
| **Senior Test Engineer** | Author / Lead SDET | *[Digitally Signed]* | Approved (Draft Submitted) | 2026-10-15 |
| **QA Automation Architect** | Test Architect | *[Pending]* | Pending Review | |
| **Engineering Manager** | Development Lead | *[Pending]* | Pending Review | |
| **Product Manager / Owner** | Baker Trader ERP PO | *[Pending]* | Pending Review | |

---
*End of Test Plan Document — Baker Trader ERP (`TP-BTERP-LOGIN-001`)*
