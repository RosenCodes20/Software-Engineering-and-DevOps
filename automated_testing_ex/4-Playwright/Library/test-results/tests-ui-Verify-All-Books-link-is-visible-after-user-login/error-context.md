# Test info

- Name: Verify "All Books" link is visible after user login
- Location: /Users/macbookpro16/Vs_code/Software-Engineering-and-DevOps/automated_testing_ex/4-Playwright/Library/tests/ui.test.js:26:1

# Error details

```
Error: page.fill: Test timeout of 30000ms exceeded.
Call log:
  - waiting for locator('input[name="email"]')

    at /Users/macbookpro16/Vs_code/Software-Engineering-and-DevOps/automated_testing_ex/4-Playwright/Library/tests/ui.test.js:29:14
```

# Page snapshot

```yaml
- banner:
  - navigation:
    - link "All Books":
      - /url: /catalog
    - link "Login":
      - /url: /login
    - link "Register":
      - /url: /register
    - text: "Welcome, {email}"
    - link "My Books":
      - /url: /profile
    - link "Add Book":
      - /url: /create
    - link "Logout":
      - /url: javascript:void(0)
- main
- contentinfo:
  - paragraph: "@Library Catalog"
```

# Test source

```ts
   1 | const { test, expect } = require('@playwright/test');
   2 |
   3 | test('Verify "All Books" link is visible', async ({ page }) => {
   4 |   await page.goto('http://localhost:3000');
   5 |
   6 |   await page.waitForSelector('nav.navbar');
   7 |
   8 |   const allBooksLink = await page.$('a[href="/catalog"]');
   9 |
   10 |   const isLinkVisible = await allBooksLink.isVisible();
   11 |   expect(isLinkVisible).toBe(true);
   12 | });
   13 |
   14 | test('Verify "Login" button is visible', async ({ page }) => {
   15 |   await page.goto('http://localhost:3000');
   16 |
   17 |   await page.waitForSelector('nav.navbar');
   18 |
   19 |   const loginButton = await page.$('a[href="/login"]');
   20 |
   21 |   const isLoginButtonVisible = await loginButton.isVisible();
   22 |
   23 |   expect(isLoginButtonVisible).toBe(true);
   24 | });
   25 |
   26 | test('Verify "All Books" link is visible after user login', async ({ page }) => {
   27 |   await page.goto('http://localhost:3000/login');
   28 |
>  29 |   await page.fill('input[name="email"]', 'peter@abv.bg');
      |              ^ Error: page.fill: Test timeout of 30000ms exceeded.
   30 |   await page.fill('input[name="password"]', '123456');
   31 |   await page.click('input[type="submit"]');
   32 |
   33 |   const allBooksLink = await page.$('a[href="/catalog"]');
   34 |   const isAllBooksLinkVisible = await allBooksLink.isVisible();
   35 |
   36 |   expect(isAllBooksLinkVisible).toBe(true);
   37 | });
   38 |
   39 | test('Login with valid credentials', async ({ page }) => {
   40 |   await page.goto('http://localhost:3000/login');
   41 |
   42 |   await page.fill('input[name="email"]', 'peter@abv.bg');
   43 |   await page.fill('input[name="password"]', '123456');
   44 |
   45 |   await page.click('input[type="submit"]');
   46 |
   47 |   await page.$('a[href="/catalog"]');
   48 |   expect(page.url()).toBe('http://localhost:3000/catalog');
   49 | });
   50 |
   51 | test('Login with empty input fields', async ({ page }) => {
   52 |   await page.goto('http://localhost:3000/login');
   53 |   await page.click('input[type="submit"]');
   54 |
   55 |   page.on('dialog', async dialog => {
   56 |       expect(dialog.type()).toContain('alert');   
   57 |       expect(dialog.message()).toContain('All fields are required!');
   58 |       await dialog.accept();
   59 |     });
   60 |
   61 |     await page.$('a[href="/login"]');
   62 |     expect(page.url()).toBe('http://localhost:3000/login');
   63 | });
   64 |
   65 | test('Add book with correct data', async ({ page }) => {
   66 |   await page.goto('http://localhost:3000/login');
   67 |
   68 |   await page.fill('input[name="email"]', 'peter@abv.bg');
   69 |   await page.fill('input[name="password"]', '123456');
   70 |
   71 |   await Promise.all([
   72 |     page.click('input[type="submit"]'), 
   73 |     page.waitForURL('http://localhost:3000/catalog')
   74 |   ]);
   75 |
   76 |   await page.click('a[href="/create"]');
   77 |
   78 |   await page.waitForSelector('#create-form');
   79 |
   80 |   await page.fill('#title', 'Test Book');
   81 |   await page.fill('#description', 'This is a test book description');
   82 |   await page.fill('#image', 'https://example.com/book-image.jpg');
   83 |   await page.selectOption('#type', 'Fiction');
   84 |
   85 |   await page.click('#create-form input[type="submit"]');
   86 |
   87 |   await page.waitForURL('http://localhost:3000/catalog');
   88 |
   89 |   expect(page.url()).toBe('http://localhost:3000/catalog');
   90 | });
   91 |
   92 | test('Add book with empty title field', async ({ page }) => {
   93 |   await page.goto('http://localhost:3000/login');
   94 |
   95 |   await page.fill('input[name="email"]', 'peter@abv.bg');
   96 |   await page.fill('input[name="password"]', '123456');
   97 |
   98 |   await Promise.all([
   99 |     page.click('input[type="submit"]'), 
  100 |     page.waitForURL('http://localhost:3000/catalog')
  101 |   ]);
  102 |
  103 |   await page.click('a[href="/create"]');
  104 |
  105 |   await page.waitForSelector('#create-form');
  106 |
  107 |   await page.fill('#description', 'This is a test book description');
  108 |   await page.fill('#image', 'https://example.com/book-image.jpg');
  109 |   await page.selectOption('#type', 'Fiction');
  110 |
  111 |   await page.click('#create-form input[type="submit"]');
  112 |
  113 |   page.on('dialog', async dialog => {
  114 |     expect(dialog.type()).toContain('alert');   
  115 |     expect(dialog.message()).toContain('All fields are required!');
  116 |     await dialog.accept();
  117 |   });
  118 |
  119 |   await page.$('a[href="/create"]');
  120 |   expect(page.url()).toBe('http://localhost:3000/create');
  121 | });
  122 |
  123 | test('Login and verify all books are displayed', async ({ page }) => {
  124 |   await page.goto('http://localhost:3000/login');
  125 |
  126 |   await page.fill('input[name="email"]', 'peter@abv.bg');
  127 |   await page.fill('input[name="password"]', '123456');
  128 |
  129 |   await Promise.all([
```