# REQUIREMENTS TRACEABILITY MATRIX (RTM)

**Project:** Baker Trader ERP — Login Module  
**Document Ref:** RTM-BTERP-LOGIN-001  
**Associated Test Plan:** `TP-BTERP-LOGIN-001`  
**Author:** Senior Test Engineer (QA Process, Product Management, Team Building)  
**Execution Target:** Playwright with TypeScript (E2E)  
**Status:** Baseline Draft — Planned  

---

## 1. Traceability Summary Matrix

| Req ID (Local) | Requirement Statement | Scenario Type | Test Case ID | Test Case Title / Description | Scope Status | Automation Feasibility | Risk | Execution Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :---: | :--- |
| `REQ-LOGIN-001` | Login Page Should be loaded Properly with title and URL. | Positive (UI) | `TC-LOGIN-001` | Verify page URL is canonical `.../login` and HTTP status is 200. | In Scope | Playwright (Automated) | Low | Not Run |
| `REQ-LOGIN-001` | Login Page Should be loaded Properly with title and URL. | Positive (UI) | `TC-LOGIN-002` | Verify page `<title>` is `Baker Trader ERP` and brand mark displays. | In Scope | Playwright (Automated) | Low | Not Run |
| `REQ-LOGIN-001` | Login Page Should be loaded Properly with title and URL. | Positive (UI) | `TC-LOGIN-003` | Verify all login form elements exist (`#login-email`, `#login-password`, submit button). | In Scope | Playwright (Automated) | Low | Not Run |
| `REQ-LOGIN-002` | Verify user can login with valid credentials. | Positive (E2E) | `TC-LOGIN-004` | Successful login with valid admin credentials redirects to `/` dashboard. | In Scope | Playwright (Automated) | Critical | Not Run |
| `REQ-LOGIN-002` | Verify user can login with valid credentials. | Positive (Security) | `TC-LOGIN-005` | Verify auth token persistence in local state/headers after successful sign-in. | In Scope | Playwright (Automated) | High | Not Run |
| `REQ-LOGIN-002` | Verify user can login with valid credentials. | Positive (Workflow) | `TC-LOGIN-006` | Verify TOTP flow transition when user account has 2FA enabled. | In Scope | Playwright (Automated) | High | Not Run |
| `REQ-LOGIN-003` | Verify user can login with invalid credentials. | Negative (Auth) | `TC-LOGIN-007` | Login with unregistered email displays "Email or password is incorrect." | In Scope | Playwright (Automated) | Critical | Not Run |
| `REQ-LOGIN-003` | Verify user can login with invalid credentials. | Negative (Auth) | `TC-LOGIN-008` | Login with valid email but wrong password displays error alert `#login-root-error`. | In Scope | Playwright (Automated) | Critical | Not Run |
| `REQ-LOGIN-003` | Verify user can login with invalid credentials. | Negative (Edge) | `TC-LOGIN-009` | Consecutive failed logins trigger HTTP 429 rate limit alert. | In Scope | Playwright (Automated) | High | Not Run |
| `REQ-LOGIN-004` | Verify user can login with empty credentials. | Negative (Validation) | `TC-LOGIN-010` | Submit form with blank email and blank password; form submission is blocked. | In Scope | Playwright (Automated) | High | Not Run |
| `REQ-LOGIN-004` | Verify user can login with empty credentials. | Negative (Validation) | `TC-LOGIN-011` | Submit form with whitespace-only inputs; validation error is raised. | In Scope | Playwright (Automated) | Medium | Not Run |
| `REQ-LOGIN-005` | Verify special characters in credentials. | Negative / Security | `TC-LOGIN-012` | Enter SQL injection pattern (`' OR '1'='1`) in email and password; verify sanitization. | In Scope | Playwright (Automated) | High | Not Run |
| `REQ-LOGIN-005` | Verify special characters in credentials. | Negative / Security | `TC-LOGIN-013` | Enter XSS script payload (`<script>alert(1)</script>`); verify no DOM script execution. | In Scope | Playwright (Automated) | High | Not Run |
| `REQ-LOGIN-005` | Verify special characters in credentials. | Positive / Edge | `TC-LOGIN-014` | Valid email with standard RFC special chars (`user+label@domain.com`) operates correctly. | In Scope | Playwright (Automated) | Medium | Not Run |
| `REQ-LOGIN-006` | Verify login with different roles. | Positive / RBAC | `TC-LOGIN-015` | Admin role logs in and accesses privileged navigation (`/admin/users`, `/masters`). | In Scope | Playwright (Automated) | High | Not Run |
| `REQ-LOGIN-006` | Verify login with different roles. | Positive / RBAC | `TC-LOGIN-016` | Non-admin role (e.g. Driver) login routes to role-specific view or displays restriction. | In Scope | Playwright (Automated) | High | Not Run |
| `REQ-LOGIN-007` | Verify login with different browsers. | Compatibility | `TC-LOGIN-017` | Execute valid and invalid login flows on Chromium desktop engine. | In Scope | Playwright (Automated) | High | Not Run |
| `REQ-LOGIN-007` | Verify login with different browsers. | Compatibility | `TC-LOGIN-018` | Execute valid and invalid login flows on Firefox & WebKit (Safari engine). | In Scope | Playwright (Automated) | High | Not Run |
| `REQ-LOGIN-008` | Verify login with different operating systems. | Compatibility | `TC-LOGIN-019` | Run Playwright test suite on Windows 11 and Ubuntu Linux CI agents. | Out of Scope | Proposed for Phase 2 | Medium | Deferred |
| `REQ-LOGIN-009` | Verify login with different devices. | Compatibility | `TC-LOGIN-020` | Responsive testing on physical mobile and tablet hardware lab. | Out of Scope | Proposed for Phase 2 | Low | Deferred |
| `REQ-LOGIN-010` | Verify login with different screen resolutions. | Responsiveness | `TC-LOGIN-021` | Viewport rendering check at 1920x1080, 1366x768, and 768x1024 tablet layout. | Out of Scope | Proposed for Phase 2 | Medium | Deferred |

---

## 2. Test Coverage Metrics

| Scope Category | Total Requirements | In-Scope Scenarios | Automated Test Cases Planned | Target Coverage | Execution Status |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Functional & UI** | 4 | 7 | 7 | 100% | Planned |
| **Authentication & RBAC** | 2 | 5 | 5 | 100% | Planned |
| **Negative & Security** | 1 | 3 | 3 | 100% | Planned |
| **Cross-Browser** | 1 | 3 | 3 | 100% | Planned |
| **Deferred (OS, Devices, Viewports)** | 3 | 3 | 0 (Phase 2) | Deferred | Deferred |
| **Total Phase 1 Commitment** | **7** | **18** | **18** | **100%** | **Ready for Scripting** |

---

## 3. Test Environment Variables Mapping

| Test Case IDs | Required Environment Variable | Fallback / Synthetic Value | Notes |
| :--- | :--- | :--- | :--- |
| `TC-LOGIN-001` to `TC-LOGIN-018` | `ERP_BASE_URL` | `https://admin-web-production-7006.up.railway.app` | Base application URL |
| `TC-LOGIN-004`, `TC-LOGIN-005` | `ERP_ADMIN_USER` | `admin@bakertrader.internal` | Valid admin test username |
| `TC-LOGIN-004`, `TC-LOGIN-005` | `ERP_ADMIN_PASSWORD` | Injected via Secret Vault | Valid admin test password |
| `TC-LOGIN-006` | `ERP_TOTP_SECRET` | Synthetic 32-char RFC base32 secret | If TOTP challenge triggered |
| `TC-LOGIN-007`, `TC-LOGIN-008` | N/A (Hardcoded Synthetic) | `unregistered@domain.invalid` | Negative synthetic credential |

---
*End of Requirements Traceability Matrix — Baker Trader ERP (`RTM-BTERP-LOGIN-001`)*
