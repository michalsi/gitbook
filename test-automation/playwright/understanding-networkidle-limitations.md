---
icon: wifi-exclamation
---

# Understanding networkidle Limitations

**TL;DR:**

* `networkidle` waits for 500ms of network inactivity
* Can be unreliable due to background requests, polling, analytics
* Use combination of strategies for robust tests

### 🚨 Why networkidle Can Be Flaky <a href="#why-networkidle-can-be-flaky" id="why-networkidle-can-be-flaky"></a>

#### 1️⃣ Common Scenarios That Cause Issues <a href="#id-1-common-scenarios-that-cause-issues" id="id-1-common-scenarios-that-cause-issues"></a>

```typescript
// This might be unreliable
async unreliableWait() {
    await page.waitForLoadState('networkidle'); // Risky
}
```

* **Background Polling:**
  * Chat systems
  * Real-time updates
  * WebSocket connections
  * Analytics pings
* **Lazy Loading:**
  * Images loading on scroll
  * Infinite scroll implementations
  * Dynamic content loading
* **Third-Party Scripts:**
  * Google Analytics
  * Advertisement networks
  * Social media widgets

### ✅ Better Approaches <a href="#better-approaches" id="better-approaches"></a>

#### 1. Combine with Specific Element Waits <a href="#id-1.-combine-with-specific-element-waits" id="id-1.-combine-with-specific-element-waits"></a>

```typescript
async reliableWait() {
    await Promise.all([
        page.waitForLoadState('networkidle'),
        page.locator('.critical-element').waitFor(),
        page.locator('.important-data').waitFor()
    ]);
}
```

#### 2. Custom Network Request Handling <a href="#id-2.-custom-network-request-handling" id="id-2.-custom-network-request-handling"></a>

```typescript
async waitForSpecificRequests() {
    await Promise.all([
        page.waitForResponse(resp => 
            resp.url().includes('/api/critical-data')
        ),
        page.locator('.content').waitFor()
    ]);
}
```

#### 3. State-Based Approach <a href="#id-3.-state-based-approach" id="id-3.-state-based-approach"></a>

```typescript
abstract class BasePage {
    async waitForReadyState() {
        await Promise.all([
            // Load state
            this.page.waitForLoadState('domcontentloaded'),
            
            // Critical elements
            ...this.getPageLoadSelectors()
                .map(selector => selector.waitFor()),
            
            // Optional: specific API call
            this.waitForCriticalData()
        ]);
    }

    protected async waitForCriticalData() {
        // Implement if needed
    }
}
```

### 🎯 Best Practices <a href="#best-practices" id="best-practices"></a>

#### 1. Define Clear Load Criteria <a href="#id-1.-define-clear-load-criteria" id="id-1.-define-clear-load-criteria"></a>

```typescript
class DashboardPage extends BasePage {
    protected getPageLoadSelectors(): Locator[] {
        return [
            this.userProfile,
            this.mainContent,
            this.navigationMenu
        ];
    }

    protected async waitForCriticalData() {
        await this.page.waitForResponse(
            resp => resp.url().includes('/api/dashboard-data')
        );
    }
}
```

#### 2. Handle Dynamic Content <a href="#id-2.-handle-dynamic-content" id="id-2.-handle-dynamic-content"></a>

```typescript
async waitForDynamicContent() {
    // Wait for initial load
    await this.waitForReadyState();

    // Handle dynamic updates
    if (await this.hasInfiniteScroll()) {
        await this.waitForFirstDataSet();
    }
}
```

### 📝 Rule of Thumb <a href="#rule-of-thumb" id="rule-of-thumb"></a>

1. Start with critical visual elements
2. Add specific API responses if needed
3. Use `networkidle` as an additional check, not primary
4. Consider your app's specific behaviour
5. Document known dynamic behaviours

### 🔍 Debugging Tips <a href="#debugging-tips" id="debugging-tips"></a>

```typescript
// Add debug logs for network activity
page.on('request', request => 
    console.log(`Request: ${request.url()}`)
);

page.on('response', response => 
    console.log(`Response: ${response.url()}`)
);
```

### 🎯 Example Implementation <a href="#example-implementation" id="example-implementation"></a>

```typescript
class ReliablePage extends BasePage {
    async waitForCompleteLoad() {
        // Primary checks
        await Promise.all([
            this.page.waitForLoadState('domcontentloaded'),
            ...this.getPageLoadSelectors()
                .map(selector => selector.waitFor())
        ]);

        // Secondary checks
        try {
            await this.page.waitForLoadState('networkidle', {
                timeout: 3000 // Short timeout
            });
        } catch (e) {
            console.warn('networkidle not reached, continuing...');
        }

        // Final verification
        await this.verifyPageState();
    }

    private async verifyPageState() {
        // Implement app-specific checks
    }
}
```
