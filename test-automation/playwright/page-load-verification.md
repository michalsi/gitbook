---
icon: hourglass-clock
---

# Page Load Verification

### Four Ways to Handle Page Load Verification <a href="#four-ways-to-handle-page-load-verification" id="four-ways-to-handle-page-load-verification"></a>

#### 1️⃣ Dedicated Method in Page Class <a href="#id-1-dedicated-method-in-page-class" id="id-1-dedicated-method-in-page-class"></a>

```typescript
class SomePage {
    async waitForPageLoad() {
        await this.header.waitFor({ state: 'visible' });
        await this.content.waitFor({ state: 'visible' });
    }
}
```

👍 Simple but limited to single page

#### 2️⃣ Generic Base Page Approach  <a href="#id-2-generic-base-page-approach-recommended" id="id-2-generic-base-page-approach-recommended"></a>

```typescript
abstract class BasePage {
    protected abstract getPageLoadSelectors(): Locator[];
    
    async waitForPageLoad() {
        await Promise.all(
            this.getPageLoadSelectors().map(selector => 
                selector.waitFor({ state: 'visible' })
            )
        );
    }
}
```

👍 Reusable, maintainable, enforces implementation

#### 3️⃣ Auto-verification in navigate() <a href="#id-3-auto-verification-in-navigate" id="id-3-auto-verification-in-navigate"></a>

```typescript
abstract class BasePage {
    async navigate() {
        await this.page.goto(this.url);
        await this.waitForPageLoad();
    }
}
```

👍 Automatic verification on navigation

#### 4️⃣ Network/Load State Approach <a href="#id-4-network-load-state-approach" id="id-4-network-load-state-approach"></a>

```typescript
abstract class BasePage {
    async navigateAndWaitForLoad() {
        await Promise.all([
            this.page.goto(this.BASE_URL + this.url),
            this.page.waitForLoadState('networkidle')
        ]);
    }
}
```

👍 Useful for:

* Complex SPAs
* Heavy API interactions
* Dynamic content loading

### &#x20;Load States Available <a href="#load-states-available" id="load-states-available"></a>

* `'domcontentloaded'` - HTML loaded and parsed
* `'load'` - page fully loaded
* `'networkidle'` - no network activity for 500ms

### 💡 When to Use What <a href="#when-to-use-what" id="when-to-use-what"></a>

* **Visual Elements** → Use approaches 1️⃣ or 2️⃣
* **Complex SPA** → Use approach 4️⃣ with `networkidle`
* **Basic Pages** → Use approach 3️⃣ with `load`
* **Mixed Requirements** → Combine approaches:

```typescript
async complexNavigate() {
    await Promise.all([
        this.page.goto(this.url),
        this.page.waitForLoadState('networkidle'),
        this.waitForPageLoad() // custom selectors
    ]);
}
```

### 🚫 Common Pitfalls <a href="#common-pitfalls" id="common-pitfalls"></a>

* Don't rely solely on `networkidle` (can be flaky)
* Avoid fixed timeouts
* Don't mix wait strategies in tests
