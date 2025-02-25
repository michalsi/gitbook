---
icon: timer
---

# Wait Strategies in Playwright Tests

{% hint style="info" %}
* Keep wait strategy consistent within a test suite
* Choose one approach and stick to it
* Mixing strategies leads to maintainability issues and flaky tests
{% endhint %}

### ❌ Bad Practice: Mixed Wait Strategies

```typescript
test('example of mixed strategies - DON'T DO THIS', async ({ page }) => {
    // Strategy 1: Direct wait
    await page.waitForTimeout(2000);
    
    // Strategy 2: Element visibility
    await page.waitForSelector('.header');
    
    // Strategy 3: Load state
    await page.waitForLoadState('networkidle');
    
    // Strategy 4: Custom assertion wait
    await expect(page.locator('.content')).toBeVisible();
});
```

### ✅ Good Practice: Consistent Strategy

```typescript
// In BasePage.ts
abstract class BasePage {
    protected abstract getPageLoadSelectors(): Locator[];
    
    async waitForPageLoad() {
        await Promise.all([
            this.page.waitForLoadState('networkidle'),
            ...this.getPageLoadSelectors().map(selector => 
                selector.waitFor({ state: 'visible' })
            )
        ]);
    }
}

// In test file
test('example of consistent strategy', async ({ page }) => {
    await homePage.navigate();
    await homePage.waitForPageLoad();
    // Rest of the test
});
```

### 🚫 Why Mixing Strategies Is Bad

1. **Inconsistent Behaviour**
   * Different wait mechanisms may conflict
   * Race conditions between waits
   * Unpredictable test execution
2. **Maintenance Nightmare**
   * Hard to debug failures
   * Difficult to update wait conditions
   * No single source of truth
3. **Performance Issues**
   * Multiple wait mechanisms stack up
   * Unnecessary waiting time
   * Increased test execution time

### 🎯 Choose One Primary Strategy

#### Option 1: Page Object Based

```typescript
class HomePage {
    async waitForLoad() {
        await this.header.waitFor();
        await this.mainContent.waitFor();
    }
}
```

#### Option 2: State Based

```typescript
class HomePage {
    async waitForLoad() {
        await this.page.waitForLoadState('networkidle');
    }
}
```

#### Option 3: Custom Assertions

```typescript
class HomePage {
    async waitForLoad() {
        await expect(this.header).toBeVisible();
        await expect(this.mainContent).toBeVisible();
    }
}
```

### 💡 Best Practices

1.  **Standardise Across Project**

    ```typescript
    // Define in one place
    abstract class BasePage {
        abstract waitForLoad(): Promise<void>;
    }
    ```
2.  **Document the Chosen Strategy**

    ```typescript
    /**
     * Project Wait Strategy:
     * - Uses Page Object pattern
     * - Combines networkidle with visual elements
     * - All pages must implement waitForLoad()
     */
    ```
3.  **Create Helper Methods**

    ```typescript
    class TestHelper {
        static async validatePageLoad(page: BasePage) {
            await page.waitForLoad();
            // Additional common validations
        }
    }
    ```

### 🎯 Remember

* Choose strategy based on application architecture
* Document the chosen approach
* Enforce through code reviews
* Create utilities to support the chosen strategy
* Consider test maintenance and debugging
