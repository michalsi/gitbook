---
icon: diagram-nested
---

# Understanding base.extend() for Custom Fixtures

{% hint style="info" %}
Playwright's `base.extend()` lets you add custom fixtures (like pre-configured page objects) to your tests. It uses TypeScript generics `<{ ... }>` to define the types of your fixtures and an object `{ ... }` to implement their setup and teardown logic. The `use` callback within each fixture's implementation is crucial for making the fixture available to your tests.
{% endhint %}

***

In Playwright, fixtures provide a powerful way to set up preconditions for your tests (e.g., launching a browser, navigating to a page, seeding a database). The `base.extend()` method takes this a step further, enabling you to define custom fixtures tailored to your application. This is particularly useful for managing page objects, API clients, and other test dependencies.

Here's a breakdown of the syntax and how it all works:

```typescript
const test = base.extend<{ /* Type Definition */ }>({ /* Implementation */ });
```

1. `const test = ...`: This line declares a new constant named `test`. This `test` will hold the enhanced version of Playwright's base `test` object, including your custom fixtures.
2.  `base.extend(...)`: This is the heart of the fixture extension. It takes two main arguments:

    a. `<{ ... }>` (Type Definition): This uses TypeScript generics to define the _types_ of your fixtures. It's like creating a blueprint. For example:

```typescript
<{
    loginPage: LoginPage;  // Fixture named 'loginPage' of type 'LoginPage'
    apiHelper: APIHelper;  // Fixture named 'apiHelper' of type 'APIHelper'
}>
```

This tells TypeScript that the extended `test` object will have two new properties: `loginPage` and `apiHelper`, with their respective types. This enables type checking and autocompletion in your tests.&#x20;

&#x20;      b. `{ ... }` (Implementation): This object provides the actual implementation of your fixtures. Each property in this object corresponds to a fixture name from the type definition. The property's value is a _factory function_ that defines how the fixture is created:

```typescript
{
    loginPage: async ({ page }, use) => { // Factory function for 'loginPage'
        const loginPage = new LoginPage(page);
        await loginPage.goto();
        await use(loginPage); // Make 'loginPage' available to the test
    },
    apiHelper: async ({}, use) => {  // Factory function for 'apiHelper'
        const apiHelper = new APIHelper();
        await use(apiHelper);      // Make 'apiHelper' available to the test
    },
}
```

Each factory function receives two arguments:

* `{ page, ... }`: This allows you to access other fixtures (like the built-in `page` fixture) that your custom fixture might depend on.
* `use` (Callback): This is a **critical** part. The `use` callback is where you pass the initialized fixture instance. The code _inside_ the `use` callback is the actual test logic that will use the fixture. This also controls the fixture's lifecycle – setup before the test and teardown afterward. 🧹

### **The** `use` Callback: Lifecycle and Availability 🔄

The `use` callback is essential for two reasons:

* **Lifecycle Management:** Code _before_ `await use(yourFixture)` handles setup, and code _after_ it handles teardown. This ensures resources are properly initialized and cleaned up, even if tests fail.
* **Fixture Availability:** Calling `await use(yourFixture)` makes `yourFixture` available to the test function.

### Example Test Usage:

```typescript
test('my test', async ({ loginPage, apiHelper }) => { // Access fixtures here
    await loginPage.login('user', 'password');
    const data = await apiHelper.fetchData();
    // ... your test assertions
});
```

By combining type definitions and factory functions with the `use` callback, `base.extend()` provides a robust and type-safe way to create and manage custom fixtures, making your Playwright tests more organised, readable, and maintainable. ✨

\
