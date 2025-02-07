---
icon: distribute-spacing-vertical
---

# Deep dive into extend(...) method

The `extend()` method in Playwright is the cornerstone of its fixture system, providing a powerful and flexible way to configure and customize your test environment. Let's take a deep dive into its inner workings:

**Purpose:**

The primary purpose of `extend()` is to create a new `test` object that inherits all the properties and functionalities of the base `test` object (provided by `@playwright/test`) while adding or overriding specific properties. This allows you to define custom fixtures, modify existing ones, or even change the default behavior of Playwright's testing functionalities.

**Signature and Type Parameters:**

The `extend()` method has the following simplified signature:

```typescript
extend<TFixtures extends Fixtures = {}>(
  fixtures: Fixtures<TFixtures, T>,
  options?: ExtendOptions
): TestType<TFixtures & T>;
```

Let's break down the type parameters:

* `TFixtures`: This represents the type of the fixtures you are adding or overriding. It defaults to an empty object `{}` if you're not extending with any new fixtures.
* `Fixtures`: This is a complex type within Playwright that represents the structure of fixtures. You don't usually interact with this directly.
* `T`: This represents the type of the base fixtures provided by Playwright (like `page`, `context`, etc.).
* `TestType<...>`: The return type of `extend()`. It's a new `test` object with the combined types of your added fixtures (`TFixtures`) and the base Playwright fixtures (`T`).

**Arguments:**

* `fixtures`: This is the most important argument. It's an object where keys are the names of your fixtures, and values are _fixture factories_. These factories are functions that define how the fixture is created and set up.
* `options` (optional): This allows you to configure aspects like fixture scopes (we'll discuss scopes later).

**Fixture Factories:**

The fixture factories are the heart of `extend()`. They have the following signature:

```typescript
async ({ page, context, ...otherFixtures }, use: (r: TFixture) => Promise<void>) => Promise<void>;
```

* `{ page, context, ...otherFixtures }`: This object gives you access to other fixtures. You can destructure it to get the specific fixtures needed within your factory.
* `use`: This is a crucial callback function. You _must_ call `use(yourFixtureInstance)` within your factory to make your fixture available to the test. The test logic that uses the fixture will be executed within the `use` callback.

**Scopes:**

Playwright fixtures have three scopes:

* `test` (default): The fixture is created anew for each test.
* `worker`: The fixture is created once per worker process. Useful for expensive setup that can be shared across tests within a worker.
* `global`: The fixture is created once for the entire test suite. Use sparingly as it can make tests less independent.

You can specify the scope in the `options` argument of `extend()` or using tags like `@test.describe.configure({ mode: 'parallel' })` for worker scope.

**How** `extend()` Works Under the Hood:

Playwright's `extend()` method utilizes a clever mechanism based on closures and dependency injection. When you call `extend()`, it creates a new `test` object that wraps the original one. This new `test` object intercepts test calls and manages the lifecycle of your defined fixtures based on their scopes and dependencies. When a test requests a fixture, Playwright checks if an instance of that fixture already exists within the appropriate scope. If not, it calls the corresponding fixture factory to create one. The `use` callback ensures that the fixture is properly initialized before the test runs and torn down afterward.

**Key Advantages of** `extend()`:

* **Type Safety:** TypeScript integration ensures that you use fixtures correctly.
* **Dependency Injection:** Easy access to other fixtures within your fixture factories.
* **Scoped Fixtures:** Control over fixture lifecycles for optimal performance and isolation.
* **Composition:** Build complex fixtures from simpler ones.
* **Extensibility:** Modify or extend existing Playwright behavior.

By understanding the inner workings of `extend()`, you can leverage its full power to create a robust, scalable, and type-safe testing framework with Playwright.
