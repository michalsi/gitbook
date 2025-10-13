---
icon: hashnode
---

# Exploring JavaScript in Node REPL: A Learning Guide

### The TypeScript Connection 🔗

TypeScript is a superset of JavaScript—all valid JavaScript is valid TypeScript (plus types). When you test JavaScript behavior in Node, you're testing exactly what your TypeScript code will do at runtime. The only difference: TypeScript's type annotations get stripped away before execution.

**What works in Node for TS learning:**

* All built-in JavaScript methods (String, Array, Object, etc.)
* Promises, async/await
* Runtime behavior and logic

**What doesn't work:**

* Type annotations (`let x: string`)
* Interfaces and type aliases
* TypeScript-specific syntax

### Quick Win: Tab Completion ⚡

The most practical discovery tool—just type and press Tab twice.

```javascript
// Type this and hit Tab:
String.
// Shows: charAt, indexOf, slice, split, substring, toLowerCase...

"hello".
// Shows all methods available on that string instance

Array.prototype.
// Shows every array method
```

This is your fastest way to discover what's available without leaving the terminal.

### Discovery Toolkit 🔍

When Tab completion isn't enough, use these introspection techniques:

#### List All Methods

```javascript
// See everything on a prototype
Object.getOwnPropertyNames(Array.prototype)
// Returns: ['length', 'constructor', 'concat', 'map', 'filter'...]

// Filter to functions only
Object.getOwnPropertyNames(String.prototype)
  .filter(p => typeof String.prototype[p] === 'function')
```

#### Check Method Signatures

```javascript
// How many parameters does it expect?
Array.prototype.map.length        // 1
Array.prototype.reduce.length     // 1 (but takes 2)
String.prototype.slice.length     // 2

// Verify method exists
'slice' in String.prototype       // true
typeof "hello".toUpperCase        // 'function'
```

### Learning Through Experimentation 💡

The REPL is your laboratory. Here's a systematic approach:

**1. Compare Similar Methods**

```javascript
const arr = [1, 2, 3, 4, 5];

arr.slice(1, 3)   // [2, 3] - creates new array
arr.splice(1, 3)  // [2, 3, 4] - modifies original

// After splice, check arr - it's changed!
```

**2. Test Edge Cases**

```javascript
// What happens with empty arrays?
[].map(x => x * 2)              // []
[].reduce((a,b) => a + b)       // Error!
[].reduce((a,b) => a + b, 0)    // 0 - needs initial value

// How are special values handled?
[1, undefined, 3].map(x => x * 2)  // [2, NaN, 6]
[1, null, 3].map(x => x * 2)       // [2, 0, 6]
```

**3. Use the `_` Variable**

```javascript
> [1,2,3].map(x => x * 2)
[2, 4, 6]
> _.filter(x => x > 2)
[4, 6]
> _
[4, 6]
```

<mark style="color:blue;">**The underscore holds your last result**</mark>—great for chaining experiments.

### Console Tools Beyond .log() 🛠️

```javascript
// View objects as tables
const users = [
  { name: 'Alice', age: 25 },
  { name: 'Bob', age: 30 }
];
console.table(users);

// Detailed object inspection
console.dir(String.prototype, { depth: null });

// Performance testing
console.time('test');
// ...your code...
console.timeEnd('test');
```

### Reusable Helper Function

Save this snippet and `.load` it when needed:

```javascript
function explore(obj) {
  const proto = Object.getPrototypeOf(obj);
  const methods = Object.getOwnPropertyNames(proto)
    .filter(p => typeof proto[p] === 'function')
    .map(name => ({
      name,
      params: proto[p].length
    }));
  console.table(methods);
}

// Usage:
// explore("hello")   → String methods
// explore([1,2,3])   → Array methods
```

### Quick Reference Card 📋

```javascript
// DISCOVERY
String.                               // Tab → show methods
Object.getOwnPropertyNames(Array.prototype)

// INSPECTION
typeof "hello".slice                  // Check if method exists
Array.prototype.map.length           // Parameter count

// REPL COMMANDS
.help        // Show REPL commands
.editor      // Multi-line mode
.load file   // Load and execute
_            // Last result

// USEFUL CHECKS
'map' in Array.prototype             // Method exists?
Array.isArray([1,2,3])              // Type checking
```

### Learning Strategy

1. Start with Tab completion to discover what exists
2. Use `getOwnPropertyNames()` for comprehensive lists
3. Test immediately with simple examples
4. Explore edge cases to understand boundaries
5. Compare related methods to see differences
6. Build reusable helper functions as you learn

The key: treat errors as teachers. When something breaks, you've discovered a boundary or learned a concept.
