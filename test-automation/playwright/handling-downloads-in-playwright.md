---
icon: desktop-arrow-down
---

# Handling Downloads in Playwright

Set up download listener BEFORE triggering download action. Use `page.waitForEvent('download')` and await the promise.

***

#### Key Pattern

```javascript
// 1. Set up listener
const downloadPromise = page.waitForEvent('download');

// 2. Trigger download
await page.getByRole('menuitem', { name: 'Export to CSV' }).click();

// 3. Wait for download
const download = await downloadPromise;
```

#### 📌 Available Download Methods

```javascript
await download.path()              // Get file path
await download.saveAs('/my/path')  // Save to location
await download.failure()           // Check for failures
await download.createReadStream()  // Create readable stream
```

#### 📁 File Handling

```typescript
// Get file from temp location
const path = await download.path();
const content = fs.readFileSync(path, 'utf8');

// Save to specific location
await download.saveAs('./downloads/myfile.csv');

// Cleanup (if needed)
await fs.promises.unlink(path);
```

#### ⚠️ Important Notes

* Always set up listener BEFORE triggering download
* Temp files are automatically cleaned up  browser context closes
* sider cleanup in your teardown if saving to custom location
* Downloads are async events
* `download` object provides multiple utility methods
