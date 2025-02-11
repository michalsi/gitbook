---
description: >-
  This document provides a detailed explanation of the `with sync_playwright()
  as p:` statement in Playwright for Python's synchronous API.
icon: arrow-right-to-arc
---

# Synchronous Playwright (Python): Mastering sync\_playwright()

### Synchronous vs. Asynchronous Playwright

Playwright offers both synchronous and asynchronous APIs. The `sync_playwright()` function is specifically for the _synchronous_ API. In synchronous mode, your Python code executes step by step, waiting for each Playwright operation to complete before proceeding to the next. This is simpler for many use cases but can be less efficient for I/O-bound operations.

### The `with` Statement: Python's Context Manager

The `with` statement in Python is a powerful construct called a _context manager_. It simplifies resource management by ensuring that resources are properly initialised and cleaned up, even in the presence of exceptions.

A context manager typically involves two key methods:

* `__enter__()`: This method is called when entering the `with` block. It's responsible for setting up the context and returning the resource to be used within the block.
* `__exit__()`: This method is called when exiting the `with` block (either normally or due to an exception). It's responsible for cleaning up the context and releasing any acquired resources.

### `sync_playwright()` and the `SyncPlaywright` Object

The `sync_playwright()` function initialises the synchronous Playwright API. It returns a `SyncPlaywright` object, which acts as the entry point for interacting with Playwright in synchronous mode. This object provides methods to launch different browser types (Chromium, Firefox, WebKit) and manage the browser context.

### Putting It Together: `with sync_playwright() as p:`

The `with sync_playwright() as p:` statement combines the power of context managers with the `sync_playwright()` initialization. Here's how it works:

1. **Initialisation:** When the `with` statement is encountered, Python calls the `__enter__()` method of the `SyncPlaywright` object returned by `sync_playwright()`. This method performs the necessary initialization of the Playwright environment.
2. **Resource Assignment:** The `__enter__()` method returns the `SyncPlaywright` object itself, which is then assigned to the variable `p`.
3. **Browser Interactions:** Within the `with` block, you use the `p` object to launch browsers, create new pages, navigate to URLs, interact with page elements, and perform other Playwright actions.
4. **Automatic Cleanup:** When the `with` block completes (either normally or due to an exception), Python calls the `__exit__()` method of the `SyncPlaywright` object (assigned to `p`). This method is crucial for cleanup. It automatically closes any open browser contexts and terminates any running browser instances.

### Why Use `with sync_playwright()`?

* **Resource Management:** It ensures that browser instances are closed correctly, preventing resource leaks (memory, file handles, etc.).
* **Exception Handling:** Even if errors occur within the `with` block, the browser context will be cleaned up properly.
* **Code Clarity:** It encapsulates the setup and teardown logic, making the code cleaner and easier to read.

### Example

```python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch() # Or p.firefox.launch() / p.webkit.launch()
    page = browser.new_page()
    page.goto("https://www.example.com")
    # ... Perform actions on the page ...

# The browser is automatically closed here, outside the 'with' block.

```

### Best Practices <a href="#best-practices" id="best-practices"></a>

* **Always use** `with sync_playwright() as p:`: This is the recommended way to use Playwright's synchronous API for robust and clean code.
* **Keep the** `with` block concise: Only include the code that directly interacts with the browser within the `with` block. This keeps the scope of the browser context well-defined.
