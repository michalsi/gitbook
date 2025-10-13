---
description: Quick reference guide for TypeScript types in Playwright test fixtures
icon: text-size
---

# Playwright's Base Test Types

### The Mystery Type Definition

When hovering over `base` in my fixtures file, I see:

```typescript
const base: TestType<
  PlaywrightTestArgs & PlaywrightTestOptions,
  PlaywrightWorkerArgs & PlaywrightWorkerOptions
>
```

This initially looks intimidating, but it's actually telling me exactly what's available in my tests.

***

### What is `base`? 🔍

In my `fixtures.ts`, I import Playwright's test function and rename it:

```typescript
import {test as base} from '@playwright/test'
```

**Why rename it?** Because I want to extend it with custom fixtures while keeping the original intact. The `base` becomes my foundation for creating an enhanced test function.

***

### Breaking Down the Type Signature

#### The Generic Structure

`TestType<A, B>` has two type parameters:

* **A**: Test-level fixtures (available in each test)
* **B**: Worker-level fixtures (shared across tests in same worker)

#### The Intersection Operator (`&`)

The `&` symbol combines multiple types together. Think of it as "AND" - you get everything from both sides.

```typescript
PlaywrightTestArgs & PlaywrightTestOptions
// Means: All args AND all options combined into one
```

***

### What's Available in Each Level? 📦

#### Test-Level (First Parameter)

**PlaywrightTestArgs** provides:

* `page` - The browser page instance
* `context` - Browser context
* `browser` - Browser instance
* `browserName` - String identifier

**PlaywrightTestOptions** provides:

* Configuration options for individual tests
* Timeout settings, screenshots, etc.

#### Worker-Level (Second Parameter)

Less commonly used, but provides shared resources across multiple tests running in the same worker process.

***

### Extending with Custom Fixtures 🛠️

This is where it gets practical. Here's how I extended `base`:

```typescript
export const test = base.extend<{
    testContext: PageObjects
}>({
    testContext: async ({page}, use) => {
        const testContext: PageObjects = {
            get summaryPage() { return new SummaryPage(page); },
            get portfolioPage() { return new PortfolioPage(page); },
            // ... more page objects
        }
        await use(testContext)
    },
})
```

**What's happening:**

1. Start with `base` (has all Playwright's stuff)
2. Add my custom `testContext` fixture
3. Now my tests have BOTH Playwright's fixtures AND my page objects

***

### Real-World Usage Example 💡

**Before custom fixtures:**

```typescript
test('add asset', async ({ page }) => {
    const summaryPage = new SummaryPage(page);
    const portfolioPage = new PortfolioPage(page);
    
    await summaryPage.navigate();
    await portfolioPage.addAsset('AAPL');
});
```

**After custom fixtures:**

```typescript
test('add asset', async ({ page, testContext }) => {
    await testContext.summaryPage.navigate();
    await testContext.portfolioPage.addAsset('AAPL');
});
```

Cleaner, less repetition, and all page objects are instantly available!

***

### Key TypeScript Concepts Recap 📝

| Concept            | Symbol | Meaning                                         |
| ------------------ | ------ | ----------------------------------------------- |
| **Generics**       | `<T>`  | Parameterized types - like variables for types  |
| **Intersection**   | `&`    | Combine multiple types together                 |
| **Type Alias**     | `as`   | Rename imports for clarity                      |
| **Type Inference** | -      | TypeScript automatically knows what's available |

***

### Quick Mental Model 🧠

Think of it like building blocks:

```
base (Playground base test)
  ↓
Comes with: page, context, browser, etc.
  ↓
I extend it
  ↓
My test = base + testContext
  ↓
Now I have everything in one place!
```

***

### Why This Matters

Understanding this type signature helps me:

* Know what's available in my tests without docs lookup
* Extend fixtures confidently
* Debug type errors when they occur
* Write more maintainable test code
