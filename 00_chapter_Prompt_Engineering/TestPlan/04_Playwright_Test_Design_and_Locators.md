# PLAYWRIGHT TEST DESIGN & LOCATOR SPECIFICATION

**Project:** Baker Trader ERP — Login Module  
**Document Ref:** DSGN-BTERP-LOGIN-001  
**Associated Test Plan:** `TP-BTERP-LOGIN-001`  
**Tooling:** Playwright Test (`@playwright/test`), TypeScript  
**Design Pattern:** Page Object Model (POM)  
**Status:** Approved Design Specification  

---

## 1. Verified Application Locators (DOM Contract)

The following locators were directly extracted and verified against the production build of Baker Trader ERP (`https://admin-web-production-7006.up.railway.app/login`):

| UI Component | Selector / Locator Strategy | Element Tag / Attribute | Usage / Purpose |
| :--- | :--- | :--- | :--- |
| **Login Page Container** | `[data-testid="login-page"]` | `<div data-testid="login-page">` | Page-level ready state assertion |
| **Email Input Field** | `#login-email` | `<input id="login-email" type="email">` | Enter user email address |
| **Password Input Field** | `#login-password` | `<input id="login-password" type="password">` | Enter user account password |
| **Toggle Password Visibility** | `button[aria-label="Show typed characters"]` | `<button aria-label="Show/Hide...">` | Toggle password masking |
| **Sign In Submit Button** | `button[type="submit"]` | `<button type="submit" aria-label="Sign in">` | Submit credential payload |
| **Authentication Error Alert** | `#login-root-error` | `<div id="login-root-error" role="alert">` | Error message container for failed logins |
| **TOTP 2FA Verification Input** | `#login-totp` | `<input id="login-totp" pattern="^\d{6}$">` | Enter 6-digit authenticator code |
| **TOTP Error Alert** | `#login-totp-error` | `<div id="login-totp-error" role="alert">` | Error message for expired/invalid 2FA code |
| **TOTP Back to Password** | `button[aria-label="Back to password"]` | `<button aria-label="Back to password">` | Return to credential view |
| **Page Title** | `Baker Trader ERP` | `<title>Baker Trader ERP</title>` | Browser tab title assertion |
| **Staff Portal Heading** | `h1` | `<h1>Sign in to your admin panel</h1>` | Main visual header |

---

## 2. Page Object Model (POM) Implementation Blueprint

```typescript
// pages/LoginPage.ts
import { Page, Locator, expect } from '@playwright/test';

export class LoginPage {
  readonly page: Page;
  readonly pageContainer: Locator;
  readonly emailInput: Locator;
  readonly passwordInput: Locator;
  readonly togglePasswordButton: Locator;
  readonly submitButton: Locator;
  readonly rootErrorAlert: Locator;
  readonly totpInput: Locator;
  readonly totpSubmitButton: Locator;
  readonly totpErrorAlert: Locator;
  readonly backToPasswordButton: Locator;
  readonly pageHeading: Locator;

  constructor(page: Page) {
    this.page = page;
    this.pageContainer = page.locator('[data-testid="login-page"]');
    this.emailInput = page.locator('#login-email');
    this.passwordInput = page.locator('#login-password');
    this.togglePasswordButton = page.locator('button[aria-label*="typed characters"]');
    this.submitButton = page.locator('button[type="submit"]').filter({ hasText: /Sign in|Signing in/ });
    this.rootErrorAlert = page.locator('#login-root-error');
    this.totpInput = page.locator('#login-totp');
    this.totpSubmitButton = page.locator('button[type="submit"]').filter({ hasText: /Verify code|Verifying/ });
    this.totpErrorAlert = page.locator('#login-totp-error');
    this.backToPasswordButton = page.locator('button[aria-label="Back to password"]');
    this.pageHeading = page.locator('h1');
  }

  /**
   * Navigates to the Baker Trader ERP login route.
   */
  async goto(): Promise<void> {
    await this.page.goto('/login', { waitUntil: 'domcontentloaded' });
    await expect(this.pageContainer).toBeVisible();
  }

  /**
   * Executes a standard credential submission.
   */
  async login(email?: string, password?: string): Promise<void> {
    if (email !== undefined) {
      await this.emailInput.fill(email);
    }
    if (password !== undefined) {
      await this.passwordInput.fill(password);
    }
    await this.submitButton.click();
  }

  /**
   * Retrieves displayed authentication error message.
   */
  async getErrorMessage(): Promise<string> {
    await expect(this.rootErrorAlert).toBeVisible();
    return (await this.rootErrorAlert.textContent()) ?? '';
  }

  /**
   * Submits 6-digit TOTP two-factor code.
   */
  async submitTotp(code: string): Promise<void> {
    await expect(this.totpInput).toBeVisible();
    await this.totpInput.fill(code);
    await this.totpSubmitButton.click();
  }
}
```

---

## 3. Sample Playwright Test Suite (`tests/login.spec.ts`)

```typescript
import { test, expect } from '@playwright/test';
import { LoginPage } from '../pages/LoginPage';

test.describe('Baker Trader ERP - Login Module (TP-BTERP-LOGIN-001)', () => {
  let loginPage: LoginPage;

  test.beforeEach(async ({ page }) => {
    loginPage = new LoginPage(page);
    await loginPage.goto();
  });

  test('TC-LOGIN-001 & 002: Login page loads properly with expected title and metadata', async ({ page }) => {
    await expect(page).toHaveTitle(/Baker Trader ERP/);
    await expect(page).toHaveURL(/\/login$/);
    await expect(loginPage.pageHeading).toHaveText('Sign in to your admin panel');
    await expect(loginPage.emailInput).toBeVisible();
    await expect(loginPage.passwordInput).toBeVisible();
    await expect(loginPage.submitButton).toBeVisible();
  });

  test('TC-LOGIN-007 & 008: User receives clear error when submitting invalid credentials', async () => {
    await loginPage.login('unregistered.user@example.invalid', 'WrongPassword!123');
    const errorText = await loginPage.getErrorMessage();
    expect(errorText).toContain('Email or password is incorrect.');
  });

  test('TC-LOGIN-010: Form prevents submission with empty credentials', async ({ page }) => {
    await loginPage.submitButton.click();
    // Verify that user remains on login page and no unexpected navigation occurs
    await expect(page).toHaveURL(/\/login$/);
  });

  test('TC-LOGIN-012: Special characters and SQL injection patterns do not trigger app crashes', async ({ page }) => {
    await loginPage.login("' OR '1'='1' --", "'; DROP TABLE users; --");
    await expect(page).toHaveURL(/\/login$/);
    await expect(loginPage.pageContainer).toBeVisible();
  });
});
```

---

## 4. Playwright Configuration Blueprint (`playwright.config.ts`)

```typescript
import { defineConfig, devices } from '@playwright/test';
import dotenv from 'dotenv';

dotenv.config();

export default defineConfig({
  testDir: './tests',
  timeout: 30000,
  expect: { timeout: 5000 },
  fullyParallel: true,
  retries: process.env.CI ? 2 : 1,
  workers: process.env.CI ? 2 : undefined,
  reporter: [
    ['html', { open: 'never', outputFolder: 'playwright-report' }],
    ['list']
  ],
  use: {
    baseURL: process.env.ERP_BASE_URL || 'https://admin-web-production-7006.up.railway.app',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
    video: 'retain-on-failure',
  },
  projects: [
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'] },
    },
    {
      name: 'firefox',
      use: { ...devices['Desktop Firefox'] },
    },
    {
      name: 'webkit',
      use: { ...devices['Desktop Safari'] },
    },
  ],
});
```

---
*End of Test Design Specification — Baker Trader ERP (`DSGN-BTERP-LOGIN-001`)*
