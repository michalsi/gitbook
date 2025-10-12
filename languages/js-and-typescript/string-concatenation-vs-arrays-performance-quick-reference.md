---
description: >-
  A practical guide to understanding why building strings with arrays is faster
  than concatenation in loops.
icon: violin
---

# String Concatenation vs Arrays: Performance Quick Reference

### ⚠️ The Problem

When building strings character-by-character in JavaScript/TypeScript, string concatenation (`+=`) in loops creates performance bottlenecks due to string immutability.

**Example of the issue:**

```typescript
function buildString(input: string): string {
    let result = '';
    for (let i = 0; i < input.length; i++) {
        result += input[i]; // Creates new string each iteration
    }
    return result;
}
```

***

### Why String Concatenation is Slow

**String Immutability:** Strings in JavaScript cannot be modified after creation. Every `+=` operation creates a brand new string object.

**The hidden cost:**

* Iteration 1: Creates string of length 1
* Iteration 2: Copies 1 char + adds 1 = 2 chars copied
* Iteration 3: Copies 2 chars + adds 1 = 3 chars copied
* Iteration n: Copies (n-1) chars + adds 1 = n chars copied

**Total operations:** 1 + 2 + 3 + ... + n = n(n+1)/2 = **O(n²) complexity**

***

### ✅ The Array Solution

Arrays are mutable, so `push()` operations are O(1) amortized. Build an array of characters, then join once at the end.

```typescript
function buildStringOptimized(input: string): string {
    const chars: string[] = [];
    for (let i = 0; i < input.length; i++) {
        chars.push(input[i]); // O(1) operation
    }
    return chars.join(''); // O(n) operation - single string creation
}
```

**Total complexity:** O(n) for pushes + O(n) for join = **O(n) overall**

***

### Side-by-Side Comparison

| Approach     | Time Complexity | Memory                         | How It Works                            |
| ------------ | --------------- | ------------------------------ | --------------------------------------- |
| String `+=`  | O(n²)           | Creates n intermediate strings | Each concatenation copies entire string |
| Array + join | O(n)            | One array + final string       | Single string creation at end           |

**Practical example from real code:**

```typescript
// Original (O(n²))
function specialOrder(inputString: string): string {
    let finalString = '';
    const inputCentre = inputString.length / 2;
    
    for (let i = 0; i < inputCentre; i++) {
        finalString += inputString[inputString.length - 1 - i]; // Slow
    }
    finalString += inputString.substring(0, Math.floor(inputCentre));
    return finalString;
}

// Optimized (O(n))
function specialOrderOptimized(inputString: string): string {
    const chars: string[] = [];
    const inputCentre = inputString.length / 2;
    
    for (let i = 0; i < inputCentre; i++) {
        chars.push(inputString[inputString.length - 1 - i]); // Fast
    }
    chars.push(inputString.substring(0, Math.floor(inputCentre)));
    return chars.join('');
}
```

***

### 💡 When Does This Matter?

**Modern JS engines** (V8, SpiderMonkey) have optimizations for string concatenation using rope data structures, which can mitigate some overhead.

**Practical thresholds:**

* **< 100 characters:** Difference negligible
* **100-1,000 characters:** Noticeable in tight loops
* **> 1,000 characters:** Significant performance difference
* **Unknown/large input:** Always use array approach

**Memory considerations:**

* String concatenation: Multiple intermediate objects → more GC pressure
* Array approach: Single array → cleaner memory profile

***

### Key Takeaways

1. String immutability makes `+=` in loops O(n²) in worst case
2. Array push + join is consistently O(n)
3. For small strings, engine optimizations make the difference minimal
4. For large or unknown input sizes, arrays are safer and faster
5. The array pattern is also more explicit about intent

**Rule of thumb:** If you're building a string in a loop and don't know the final length, use arrays.
