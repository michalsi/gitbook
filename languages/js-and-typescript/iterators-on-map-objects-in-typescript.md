---
icon: arrow-down-square-triangle
---

# Iterators on Map Objects in TypeScript

Understanding Iterators on Map Objects in TypeScript

## Overview

Working with the `Map` object in TypeScript can be confusing if you’re coming from arrays and expect familiar methods like `.map()` or `.reduce()`. This quick reference explains how iteration works on `Map`, why it behaves differently from arrays, and how to achieve similar transformations efficiently.

## 1. What Happens When You Iterate a Map ⚙️

A `Map` in JavaScript/TypeScript is **iterable**, and its default iterator returns `[key, value]` pairs.

```ts
const map = new Map([
  ['a', 1],
  ['b', 2],
  ['c', 3]
]);

for (const [key, value] of map) {
  console.log(key, value);
}
// a 1
// b 2
// c 3
```

Key takeaway: when you iterate over a `Map`, you’re iterating over entries, not just values or keys.

## 2. Missing Array Methods 🚫

`Map` does **not** include methods like `.map()`, `.filter()`, or `.reduce()`. Those methods belong to the **Array prototype**, not to `Map`.

```ts
map.map(...) // ❌ TypeError — not a function
```

However, `Map` does have its own `.forEach()` method, though it behaves slightly differently:

```ts
map.forEach((value, key) => {
  console.log(`${key}: ${value}`);
});
```

## 3. Using Array Methods on Maps ✅

To use functions like `.map()` or `.reduce()`, convert your map entries into an array first.

#### Example 1: Transforming Values

```ts
const doubled = new Map(
  [...map].map(([key, value]) => [key, value * 2])
);
// Result: Map(3) { 'a' => 2, 'b' => 4, 'c' => 6 }
```

#### Example 2: Reducing Values

```ts
const sum = [...map].reduce((acc, [, value]) => acc + value, 0);
// Result: 6
```

This pattern—convert to array → transform → reconstruct as Map—is both expressive and TypeScript-friendly.

## 4. Why Use Map Iterators? 💡

Iterating a `Map` directly can be **faster and more memory-efficient** since it avoids array creation. It’s best when:

* You’re only reading or processing entries
* You don’t need to transform the data into a new structure

Example:

```ts
for (const [key, value] of map) {
  console.log(`${key}: ${value}`);
}
```

## 5. Mutating Values Inside forEach 🔄

When you modify a value inside `map.forEach()`, the effect depends on the type of value stored in the `Map`.

#### 🧩 1. Primitives

For primitive values (numbers, strings, booleans), changes **won’t** persist.

```ts
const map = new Map([
  ['a', 1],
  ['b', 2]
]);

map.forEach((value, key) => {
  value = value * 10; // only changes the local variable
});

console.log(map.get('a')); // still 1
```

💡 Primitives are passed **by value**, so modifications affect only the local copy.

#### 🧱 2. Objects or Arrays

For reference types, mutating their _contents_ **will** persist, since objects are passed **by reference**.

```ts
const map = new Map([
  ['x', { count: 1 }],
  ['y', { count: 2 }]
]);

map.forEach(value => {
  value.count += 10; // modifies the same object reference
});

console.log(map.get('x')); // { count: 11 }
```

But if you **reassign** the object entirely, the Map won’t update:

```ts
map.forEach((value, key) => {
  value = { count: value.count * 2 }; // new object, not linked
});
console.log(map.get('x')); // still { count: 11 }
```

#### ✅ To Actually Update Stored Values

Use `map.set()` for explicit updates:

```ts
map.forEach((value, key) => {
  map.set(key, value * 10);
});
```

#### Summary Table

| Value Type                          | Mutation Persists? | Example                  |
| ----------------------------------- | ------------------ | ------------------------ |
| Primitive (number, string, boolean) | ❌ No               | `value = 5`              |
| Object / Array (mutate properties)  | ✅ Yes              | `value.x = 10`           |
| Object / Array (reassign)           | ❌ No               | `value = {x:10}`         |
| Explicit `map.set()`                | ✅ Yes              | `map.set(key, newValue)` |

## 6. Practical Summary 🧠

| Task                                              | Recommended Approach               |
| ------------------------------------------------- | ---------------------------------- |
| Iterate all entries                               | `for..of`, `map.entries()`         |
| Iterate keys                                      | `map.keys()`                       |
| Iterate values                                    | `map.values()`                     |
| Apply transformations (`map`, `filter`, `reduce`) | Convert to array first: `[...map]` |
| In-place iteration                                | `map.forEach((value, key) => …)`   |

## 7. Optional Enhancement 🔧

You can define small utility functions or prototypes to extend `Map` behavior safely (if appropriate for your project):

```ts
function mapMap<K, V, R>(m: Map<K, V>, fn: (v: V, k: K) => R): Map<K, R> {
  return new Map([...m].map(([k, v]) => [k, fn(v, k)]));
}
```

This helps you reuse array-like transformations while preserving type safety.

## Final Thoughts

Iterators make `Map` flexible and efficient, but it doesn’t mirror array APIs. Converting between `Map` and arrays gives you the best of both worlds—functional transformations and efficient key-value storage. Keep this distinction in mind when designing data manipulation flows.
