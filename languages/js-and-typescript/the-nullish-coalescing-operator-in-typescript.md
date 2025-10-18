---
icon: circle-question
---

# The Nullish Coalescing Operator (??) in TypeScript

## Overview

The `??` operator—known as the **nullish coalescing operator**—is one of the most useful modern additions to JavaScript and TypeScript. It allows you to provide **default values** only when something is truly missing (`null` or `undefined`), without mistakenly overwriting legitimate “falsy” values like `0` or an empty string.

A great example of its practical use appears in this pattern:

```ts
index.set(word, (index.get(word) ?? 0) + 1);
```

Let’s unpack exactly what’s happening here 👇

## 1. Step-by-Step Breakdown ⚙️

#### The code

```ts
index.set(word, (index.get(word) ?? 0) + 1);
```

#### Explanation

1. **`index.get(word)`**\
   Retrieves the current value associated with the key `word` from the `Map` named `index`.
   * If the key exists → returns its value (e.g., `3`)
   * If not → returns `undefined`
2.  **`?? 0` — the nullish coalescing operator**\
    This ensures that when the value is `null` or `undefined`, it falls back to `0`.\
    It does _not_ trigger on other falsy values like `0`, `false`, or `''`.

    ```ts
    undefined ?? 0 // → 0
    null ?? 0      // → 0
    5 ?? 0         // → 5
    0 ?? 0         // → 0 ✅
    ```
3. **Adding +1**\
   The expression `(index.get(word) ?? 0) + 1` calculates the next count value, treating missing keys as zero.
4. **`index.set(word, …)`**\
   Updates or inserts the new count into the map.

***

## 2. Expanded Equivalent Example 🧩

To see what this line does under the hood:

```ts
index.set(word, (index.get(word) ?? 0) + 1);
```

Is roughly equivalent to:

```ts
let count = index.get(word);
if (count === undefined || count === null) {
  count = 0;
}
index.set(word, count + 1);
```

Cleaner, shorter, and safer — that’s what `??` gives you.

***

## 3. Difference Between `??` and `||` ⚠️

At first glance, you might think the following are equivalent:

```ts
index.set(word, (index.get(word) || 0) + 1);
```

But they’re **not** the same.

#### Key difference:

* `||` returns the right-hand side if the left-hand side is **falsy**
* `??` returns the right-hand side only if the left-hand side is **null or undefined**

#### Comparison

<table><thead><tr><th width="249">Value</th><th>Using || 0</th><th>Using ?? 0</th></tr></thead><tbody><tr><td><code>undefined</code></td><td><code>0</code></td><td><code>0</code></td></tr><tr><td><code>null</code></td><td><code>0</code></td><td><code>0</code></td></tr><tr><td><code>0</code></td><td><code>0 → replaced ❌</code></td><td><code>0 → preserved ✅</code></td></tr><tr><td><code>false</code></td><td><code>0 → replaced ❌</code></td><td><code>false → preserved ✅</code></td></tr><tr><td><code>''</code> <em>(empty string)</em></td><td><code>0 → replaced ❌</code></td><td><code>'' → preserved ✅</code></td></tr></tbody></table>

***

## 4. Practical Example 💡

Imagine a frequency counter:

```ts
const index = new Map<string, number>();
const words = ['apple', 'banana', 'apple'];

for (const word of words) {
  index.set(word, (index.get(word) ?? 0) + 1);
}

console.log(index);
// Map(2) { 'apple' => 2, 'banana' => 1 }
```

This works perfectly even when a key has a count of `0`, ensuring that legitimate zeroes are not overwritten.

***

## 5. Summary 🧠

| Operator                  | Fallback triggers when         | Safe for `0`?   | Example                                     |
| ------------------------- | ------------------------------ | --------------- | ------------------------------------------- |
| `??` (nullish coalescing) | Value is `null` or `undefined` | ✅ Yes           | `(undefined ?? 5) → 5`                      |
| \`                        |                                | \` (logical OR) | Value is _falsy_ (`0`, `''`, `false`, etc.) |

✅ Use `??` for **default initialization** (e.g., counters, configs, safe object reads).\
⚠️ Use `||` only when you truly mean _“any falsy value should trigger the fallback.”_

***

## Final Thoughts

The nullish coalescing operator makes your intent explicit:

> “Use this default only when the value doesn’t exist.”

In `Map` operations, configuration defaults, or counting logic, it’s both cleaner and safer than using logical OR. Embrace `??` whenever you need precise, predictable default behavior in TypeScript.
