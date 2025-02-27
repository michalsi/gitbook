---
icon: ellipsis-stroke
---

# spread syntax (...)

{% hint style="info" %}
The spread syntax (`...`) expands iterables (arrays, objects, strings) into individual elements. Useful for shallow copying, merging, and function arguments.
{% endhint %}

**Spread Syntax (...)** ➡️

* **Purpose:** Expands an iterable into its individual elements. Think of it as "unpacking" the iterable.
* **Iterables:** Works with arrays, objects, strings, and other iterable data structures.
* **Use Cases:**
  *   **Shallow Copying:** Creates a new object/array with the same top-level properties/elements. Nested objects/arrays are still referenced, not copied.

      ```js
      const original = [1, 2, [3, 4]];
      const copy = [...original];

      copy[0] = 5; // original remains unchanged: [1, 2, [3, 4]]
      copy[2][0] = 6;  // original[2][0] IS changed because of nested reference!: [1, 2, [6, 4]]
      console.log(original); // Output: [1, 2, [6, 4]]
      console.log(copy);    // Output: [5, 2, [6, 4]]
      ```
  * **Merging/Combining:** Combines multiple arrays or objects. Later properties overwrite earlier ones in objects./code

```javascript
const arr1 = [1, 2];
const arr2 = [3, 4];
const combined = [...arr1, ...arr2]; // [1, 2, 3, 4]
const obj1 = { a: 1, b: 2 };
const obj2 = { b: 3, c: 4 };
const merged = { ...obj1, ...obj2 }; // { a: 1, b: 3, c: 4 }
```

*   **Function Arguments:** Passes array/object elements as individual arguments to a function.

    ```javascript
    function myFunc(a, b, c) { /* ... */ }
    const args = [1, 2, 3];
    myFunc(...args); // Equivalent to myFunc(1, 2, 3)
    ```
*   **String Manipulation:** Expands a string into individual characters.

    ```javascript
    const str = "hello";
    const charArray = [...str]; // ['h', 'e', 'l', 'l', 'o']
    ```
* Shallow Copy Nuances: Remember that nested objects and arrays are referenced, not copied, in a shallow copy. Modifying a nested element in the copy will affect the original. Use deep copy methods (e.g., `JSON.parse(JSON.stringify(obj))`) if you need fully independent copies of nested structures. However, deep copies can be less performant. Choose the approach that best suits your needs. ⚠️
* **Alternatives:** `Object.assign()` can also be used for merging objects, but spread syntax is generally more concise and readable. `Array.concat()` merges arrays. However, spread syntax is more versatile as it works with any iterable.

\
