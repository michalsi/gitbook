---
icon: grid-2-plus
---

# Effective Use of Fixtures

{% hint style="info" %}
Use modular and composable fixtures in Playwright for scalable and maintainable tests. Separate concerns, leverage configuration files, and use environment variables for flexible setups.
{% endhint %}

***

## **1. Modularise Fixtures**

* **Separate Fixtures for Each Concern:** Create distinct fixtures for different components like page objects, APIs, and databases. This prevents repetition and makes each fixture responsible for a specific part of the setup.

```typescript
// PageObject Fixture
const pageFixture = base.extend<{ pageObj: PageObject }>({
  pageObj: async ({ page }, use) => {
    const pageObj = new PageObject(page);
    await pageObj.setup(); // e.g., navigate to page, set initial state
    await use(pageObj);    // Make pageObj available to tests
    await pageObj.cleanup(); // e.g., clear state, close connections
  },
});

// API Client Fixture
const apiFixture = base.extend<{ apiClient: ApiClient }>({
  apiClient: async ({}, use) => {
    const apiClient = new ApiClient();
    await apiClient.authenticate(); // e.g., login, set tokens
    await use(apiClient);           // Make apiClient available to tests
  },
});

// Database Fixture
const dbFixture = base.extend<{ db: Database }>({
  db: async ({}, use) => {
    const db = new Database();
    await db.connect();   // Establish database connection
    await use(db);        // Provide db instance to tests
    await db.disconnect(); // Clean up connection
  },
});
```

## **2. Compose Fixtures**

* **Combine Fixtures for Specific Test Suites:** Use fixture composition to include only the necessary fixtures for a given test suite. This ensures tests have access to only the resources they need, improving performance and reducing overhead.

```typescript
const test = base
  .extend(pageFixture)
  .extend(apiFixture)
  .extend(dbFixture);

test('complex test', async ({ pageObj, apiClient, db }) => {
  // Use fixtures to perform actions
  await pageObj.performAction(); // Interact with UI
  const data = await apiClient.fetchData(); // Fetch data via API
  await db.saveData(data); // Save data to the database
});
```

## **3. Use Configuration Files**

* **Centralise Shared Settings:** Define common configurations such as base URLs, timeouts, and other settings in a configuration file (`playwright.config.ts`). This helps manage environment-specific settings and reduces duplication.

```typescript
// playwright.config.ts
import { PlaywrightTestConfig } from '@playwright/test';

const config: PlaywrightTestConfig = {
  use: {
    baseURL: 'https://example.com',
    viewport: { width: 1280, height: 720 },
    // Add other default settings here
  },
  projects: [
    // Define environment-specific or parallel test configuration
  ],
};

export default config;
```

## **4. Leverage Environment Variables**

* **Runtime Configuration Flexibility:** Use environment variables to inject configurations that can change between environments. This is useful for sensitive information like API keys or environment-specific URLs.

```sh
# Example of setting environment variables
export API_KEY="your_api_key"
export BASE_URL="https://staging.example.com"
```

In your tests, access these variables using `process.env`:

```typescript
const apiKey = process.env.API_KEY;
const baseUrl = process.env.BASE_URL;
```

## **5. Adopt a Layered Approach**

* **Maintain Clear Separation of Concerns:** Organize your tests and application code in layers, such as UI interactions, business logic, and data access layers. This separation helps in maintaining clean and testable code.
  * **UI Layer:** Interacts with the application's user interface.
  * **Business Logic Layer:** Contains the core logic of the application.
  * **Data Layer:** Manages data interactions, such as CRUD operations.

## Understanding the `use` Function in Playwright Fixtures <a href="#understanding-the-use-function-in-playwright-fixtures" id="understanding-the-use-function-in-playwright-fixtures"></a>

The `use` function is an integral part of how Playwright handles fixtures. It allows you to define setup and teardown logic around the lifecycle of a fixture. Here’s a detailed explanation:

**Role of** `use`:

* The `use` function is a callback that you call with the fixture instance (e.g., a page object, API client).
* It signifies to Playwright that the setup for this fixture is complete, and it can now be used in the tests.
* After `use` is called, the code following it is run after the test finishes, performing any necessary cleanup (teardown).

**Example Workflow:**

1. **Setup Phase:**
   * Initialise resources required for the tests (e.g., navigate to a page, establish a database connection).
   * Call `use` with the initialized resource to make it available to the test.
2. **Test Execution:**
   * The test executes using the resource provided by `use`.
3. **Teardown Phase:**
   * Code following the `use` function is executed after the test, allowing you to clean up resources (e.g., close connections, reset state).

**Example in Code:**

```typescript
const test = base.extend<{ pageObj: PageObject }>({
  pageObj: async ({ page }, use) => {
    // Setup phase
    const pageObj = new PageObject(page);
    await pageObj.goto();  // Navigate to the initial URL
    await pageObj.setupInitialState(); // Set initial state

    // Make the pageObj available to tests
    await use(pageObj);

    // Teardown phase
    await pageObj.cleanup(); // Clean up any changes made during the test
  },
});

test('should perform an action', async ({ pageObj }) => {
  await pageObj.performAction(); // Use the fixture in the test
});
```

**Key Points to Remember:**

* **Asynchronous Nature:** The `use` function must be awaited, as it typically involves asynchronous operations. This ensures that the test does not proceed until the fixture is fully set up.
* **Lifecycle Management:** The `use` function helps in managing the lifecycle of resources, ensuring they are properly initialized and disposed of, which is crucial for test reliability and resource efficiency.
* **Isolation:** By encapsulating setup and teardown logic, `use` helps isolate tests, reducing side effects between tests.

***

🔧 **Best Practices Recap:**

* **Modularize:** Break down fixtures by functionality for clarity and reusability.
* **Compose:** Combine necessary fixtures for specific test suites to reduce overhead.
* **Centralize:** Use configuration files for shared settings to avoid duplication.
* **Flexibility:** Utilize environment variables for dynamic configurations.
* **Structure:** Maintain a layered architecture for better separation of concerns.

By implementing these strategies, you will create a robust and efficient testing framework in Playwright that can adapt to changes and scale with your application's needs.
