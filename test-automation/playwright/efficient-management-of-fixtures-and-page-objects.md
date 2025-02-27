---
icon: industry
---

# Efficient Management of Fixtures and Page Objects



### TL;DR

{% hint style="info" %}
* **Lazy Initialization**: Use getters to create page objects only when needed, optimising resource use.
* **Factory Method Pattern**: Centralise page object creation for better control and flexibility.
* **Conditional Initialisation**: Initialise page objects based on specific test conditions.
{% endhint %}

***

When working with Playwright, managing page objects efficiently is essential for maintaining a scalable and performant test suite. Here are some strategies to optimise the initialisation and usage of page objects, ensuring that your tests remain efficient and easy to manage.

### Lazy Initialization: On-Demand Object Creation

#### 🔍 What is it?

Lazy initialisation means creating objects only when they are accessed for the first time. This approach helps save memory and reduces unnecessary processing.

#### 💡 How to Implement

First, we define our `fixtures.ts` file where we will use getters to lazily initialise page objects. This reduces unnecessary initialization and optimises resource usage.

```typescript
// fixtures.ts
import { test as base } from '@playwright/test';
import { AssetPage } from './pages/AssetPage';
import { ImportPage } from './pages/ImportPage';
// Import other page objects as needed

interface PageObjects {
    assetPage: AssetPage;
    importPage: ImportPage;
    // Add other page objects
}

export const test = base.extend<{
    testContext: PageObjects
}>({
    testContext: async ({ page }, use) => {
        const testContext: PageObjects = {
            get assetPage() {
                return new AssetPage(page);
            },
            get importPage() {
                return new ImportPage(page);
            },
            // Define other getters for additional page objects
        };
        await use(testContext);
    },
});
```

#### Example Test: Using the Lazily Initialized Page Objects

Now, let's see how this setup is used in a test. We'll create a test that interacts with both the `AssetPage` and `ImportPage` objects.

```typescript
// AssetImportTest.spec.ts
import { test } from '../fixtures';
import { expect } from '@playwright/test';

test('Verify asset navigation and import functionality', async ({ testContext }) => {
    // Lazy initialization happens here when assetPage is first accessed
    const assetPage = testContext.assetPage;
    await assetPage.navigate();
    await assetPage.acceptCookies();

    // Perform actions specific to AssetPage
    await assetPage.performBuyTransaction(10, 'Test Transaction');
    await expect(assetPage.getTransactionNotes()).toContainText('Test Transaction');

    // Lazy initialization happens here when importPage is first accessed
    const importPage = testContext.importPage;
    await importPage.navigate();
    await importPage.acceptCookies();

    // Perform actions specific to ImportPage
    await importPage.clickBrokerMBank();
    await importPage.uploadFile('path/to/transactions.csv');
    await expect(importPage.getReviewHeader()).toContainText('Review items to be imported');
});
```

#### Explanation

1. **Lazy Initialization**:
   * The `get assetPage()` and `get importPage()` getters in `fixtures.ts` ensure that the respective page objects are only created when they are accessed for the first time in a test.
   * This means if a test only needs `assetPage`, `importPage` won't be initialized, saving resources.
2. **Test Logic**:
   * In `AssetImportTest.spec.ts`, both `assetPage` and `importPage` are accessed when needed. This triggers their initialization via the getters.
   * The test demonstrates interacting with both page objects, showing how you can seamlessly transition between them in a test.
3. **Benefits**:
   * This approach ensures that only the necessary objects are initialized for a given test, reducing memory usage and potentially speeding up test execution.
   * It also keeps the code clean and organised, as the initialization logic is encapsulated within the getters.

### Factory Method Pattern: Centralized Creation

#### 🔍 What is it?

The factory method pattern centralises the creation of objects, allowing you to manage instantiation logic in one place, making it easier to handle dependencies or configurations.

#### 💡 How to Implement

Create a factory class responsible for creating page objects.

```typescript
class PageObjectFactory {
    private page: Page;

    constructor(page: Page) {
        this.page = page;
    }

    createAssetPage() {
        return new AssetPage(this.page);
    }

    createImportPage() {
        return new ImportPage(this.page);
    }

    // Additional methods for other page objects
}

// Usage in fixtures.ts
export const test = base.extend<{
    testContext: { factory: PageObjectFactory }
}>({
    testContext: async ({ page }, use) => {
        const factory = new PageObjectFactory(page);
        await use({ factory });
    },
});

// In tests
test('example test', async ({ testContext }) => {
    const assetPage = testContext.factory.createAssetPage();
    await assetPage.navigate();
    // ...
});
```

#### 🎯 Benefits

* **Encapsulation**: Keeps creation logic centralized, making maintenance easier.
* **Flexibility**: Allows for injecting different dependencies or configurations as needed.

### Conditional Initialisation: Context-Aware Instantiation

#### 🔍 What is it?

Initialise page objects conditionally, based on specific test requirements or scenarios.

#### 💡 How to Implement

Use logical conditions to determine when to initialize certain page objects.

```typescript
test('conditional test', async ({ testContext }) => {
    if (someCondition) {
        const importPage = testContext.factory.createImportPage();
        await importPage.navigate();
    }
    // Proceed based on the test scenario
});
```

#### 🎯 Benefits

* **Resource Management**: Only instantiate objects that are necessary for the test.
* **Adaptability**: Easily adjust to different test scenarios without bloating the test setup.

### Conclusion

Efficient management of page objects using lazy initialisation, factory patterns, and conditional creation can greatly enhance the performance and scalability of your Playwright test suite. By implementing these strategies, you'll ensure that your tests remain robust, maintainable, and ready to grow as your application evolves.
