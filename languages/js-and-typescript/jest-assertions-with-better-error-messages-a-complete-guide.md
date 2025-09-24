---
icon: glasses-round
---

# Jest Assertions with Better Error Messages: A Complete Guide

When writing tests with Jest and TypeScript, you might run into a frustrating limitation: Jest's `expect` function doesn't accept a custom error message as a second argument. This becomes particularly annoying when you're testing API endpoints and want to include response data in your error messages for better debugging.

### The Problem

Let's say you're testing a healthcheck endpoint and want to include the response data when the test fails:

```typescript
test('Healthcheck endpoint should return 200', async () => {
  const response = await axios.get('http://localhost:5240/healthcheck');
  
  // ❌ This won't compile in TypeScript + Jest
  expect(response.status, `Response data: ${JSON.stringify(response.data)}`).toBe(200);
});
```

TypeScript will complain because Jest's `expect` only takes one argument: the actual value. So how do we get better error messages with debugging information?

### Solution 1: Manual Check with Template Strings ✅

The most straightforward approach is to perform a manual check before the assertion:

```typescript
test('Healthcheck endpoint should return 200', async () => {
  const response = await axios.get('http://localhost:5240/healthcheck');
  const status = response.status;
  const data = JSON.stringify(response.data);

  if (status !== 200) {
    throw new Error(`Healthcheck failed: Got status ${status}, expected 200. Response data: ${data}`);
  }

  expect(status).toBe(200);
});
```

**Pros:**

* Simple and straightforward
* Preserves all the debug information you need
* No additional dependencies
* Clear error messages

**Cons:**

* Requires manual condition checking
* Slightly more verbose

### Solution 2: Use Node's `assert` Module ✅

Node.js's built-in `assert` module (or libraries like Chai) support custom error messages:

```typescript
import assert from 'assert';

test('Healthcheck endpoint should return 200', async () => {
  const response = await axios.get('http://localhost:5240/healthcheck');
  
  assert.strictEqual(
    response.status,
    200,
    `Healthcheck failed: Got status ${response.status}, expected 200. Response data: ${JSON.stringify(response.data)}`
  );
});
```

**Pros:**

* Clean, single-line assertion
* Built-in support for custom messages
* Part of Node.js standard library

**Cons:**

* Mixed assertion libraries in your test suite
* Different API from Jest's expect

### Solution 3: Custom Jest Matchers ✅

For the cleanest and most reusable approach, you can extend Jest with custom matchers:

```typescript
// Setup (in your test setup file or at the top of your test)
expect.extend({
  toBe200(received, data) {
    const pass = received === 200;
    if (pass) {
      return {
        message: () => `expected ${received} not to be 200`,
        pass: true,
      };
    } else {
      return {
        message: () =>
          `Healthcheck failed: Got status ${received}, expected 200. Response data: ${JSON.stringify(data)}`,
        pass: false,
      };
    }
  },
});

// Add TypeScript declaration
declare global {
  namespace jest {
    interface Matchers<R> {
      toBe200(data?: any): R;
    }
  }
}

// Usage in your test
test('Healthcheck endpoint should return 200', async () => {
  const response = await axios.get('http://localhost:5240/healthcheck');
  expect(response.status).toBe200(response.data);
});
```

**Pros:**

* Very clean test code
* Reusable across your test suite
* Consistent with Jest's API
* Type-safe with proper TypeScript declarations

**Cons:**

* Requires initial setup
* More complex for simple use cases

### Generic Custom Matcher

You can also create a more generic custom matcher for any status code:

```typescript
expect.extend({
  toBeStatusWithData(received, expectedStatus, data) {
    const pass = received === expectedStatus;
    if (pass) {
      return {
        message: () => `expected ${received} not to be ${expectedStatus}`,
        pass: true,
      };
    } else {
      return {
        message: () =>
          `API call failed: Got status ${received}, expected ${expectedStatus}. Response data: ${JSON.stringify(data)}`,
        pass: false,
      };
    }
  },
});

// Usage
expect(response.status).toBeStatusWithData(200, response.data);
expect(response.status).toBeStatusWithData(404, response.data);
```

### When to Use Each Approach

* **Use Solution 1** (manual check) when you need a quick fix or are dealing with complex conditional logic
* **Use Solution 2** (assert) when you want consistent custom error messages but don't mind mixing assertion libraries
* **Use Solution 3** (custom matchers) when you want the cleanest API and plan to reuse the pattern across multiple tests

### Conclusion

While Jest's limitation on custom error messages can be frustrating, there are several elegant solutions to get better debugging information in your test failures. Choose the approach that best fits your team's preferences and testing patterns.

The key is ensuring that when tests fail, you have enough information to quickly understand what went wrong without having to re-run the test or add temporary logging statements.
