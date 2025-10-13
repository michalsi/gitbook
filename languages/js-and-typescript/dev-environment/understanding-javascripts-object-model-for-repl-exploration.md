---
icon: draw-square
---

# Understanding JavaScript's Object Model for REPL exploration

### What Are Prototypes?

Every JavaScript object has a hidden internal link to another object called its **prototype**. When you try to access a property or method on an object, JavaScript first looks at the object itself. If not found, it looks at the object's prototype. If still not found, it looks at the prototype's prototype, and so on—this is called the **prototype chain**.

```javascript
const arr = [1, 2, 3];

// When you call arr.map(...), JavaScript:
// 1. Checks if 'map' exists on arr itself → No
// 2. Checks arr's prototype (Array.prototype) → Yes, found!
// 3. Executes Array.prototype.map with arr as 'this'
```

#### The Constructor.prototype Pattern

Constructor functions (like `Array`, `String`, `Object`) have a special property called `.prototype`. This is NOT the constructor's own prototype—it's the object that will become the prototype for all instances created with that constructor.

```javascript
// Array is a constructor function
typeof Array                    // 'function'

// Array.prototype is an object containing shared methods
typeof Array.prototype          // 'object'

// When you create an array:
const myArr = [1, 2, 3];

// Its internal prototype links to Array.prototype:
Object.getPrototypeOf(myArr) === Array.prototype  // true
```

#### Why This Matters for Discovery

This design means **all array methods exist in one place**: `Array.prototype`. They're not copied to each array instance—they're shared. This is memory efficient and gives us a single location to inspect.

```javascript
// Every array shares the same methods
const arr1 = [1, 2];
const arr2 = [3, 4];

Object.getPrototypeOf(arr1) === Object.getPrototypeOf(arr2)  // true
// Both point to Array.prototype
```

### Discovery Toolkit 🔍 - How These Actually Work

#### Object.getOwnPropertyNames(obj) - Deep Dive

**What it does:** Returns an array of ALL property names found directly on an object, including non-enumerable properties.

**How it works:** JavaScript objects have two types of properties:

* **Own properties**: Exist directly on the object
* **Inherited properties**: Come from the prototype chain

```javascript
const arr = [1, 2, 3];

// Own properties (on THIS specific array)
Object.getOwnPropertyNames(arr);
// ['0', '1', '2', 'length']
// These are the indices and length specific to this array

// Prototype properties (shared by ALL arrays)
Object.getOwnPropertyNames(Object.getPrototypeOf(arr));
// ['constructor', 'concat', 'map', 'filter', 'reduce', ...]
// These methods exist on Array.prototype

// Shortcut: directly inspect Array.prototype
Object.getOwnPropertyNames(Array.prototype);
// Same result - the blueprint for all arrays
```

**Why it's better than Object.keys():**

```javascript
// Object.keys only returns ENUMERABLE properties
Object.keys(Array.prototype);
// [] or very few results

// getOwnPropertyNames returns ALL properties
Object.getOwnPropertyNames(Array.prototype);
// Complete list including non-enumerable methods
```

#### The 'in' Operator - Prototype Chain Traversal

**What it does:** Tests whether a property exists anywhere in an object's prototype chain.

**How it works:** Traverses the prototype chain until it finds the property or reaches the end (null).

```javascript
const arr = [1, 2, 3];

// Check if property exists (anywhere in chain)
'map' in arr;                    // true (found in Array.prototype)
'length' in arr;                 // true (own property)
'toString' in arr;               // true (from Object.prototype)

// Compare with hasOwnProperty (only checks own properties)
arr.hasOwnProperty('map');       // false (not own property)
arr.hasOwnProperty('length');    // true (own property)

// This is why 'in' is useful for discovery:
// It finds methods even if they're inherited
```

**Practical use for learning:**

```javascript
// Check if a method exists before trying it
if ('flatMap' in Array.prototype) {
  console.log('flatMap is available');
} else {
  console.log('flatMap not supported in this environment');
}
```

#### Function.length - Understanding Arity

**What it is:** Every function object has a `length` property that indicates its **arity** (number of formal parameters).

**How it works:** JavaScript stores metadata about functions, including parameter count. This is set when the function is defined.

```javascript
function example(a, b, c) {
  return a + b + c;
}
example.length;  // 3

// Built-in methods also have this:
Array.prototype.map.length;      // 1 (expects callback)
Array.prototype.filter.length;   // 1 (expects callback)
Array.prototype.reduce.length;   // 1 (callback required, initialValue optional)
String.prototype.slice.length;   // 2 (start and end)
```

**Limitations to know:**

```javascript
// Rest parameters don't count
function withRest(a, b, ...rest) {}
withRest.length;  // 2 (only a and b)

// Default parameters count only up to the first one
function withDefaults(a, b = 10, c) {}
withDefaults.length;  // 1 (only a, b has default)
```

**Why it's useful:** Gives you a quick hint about required parameters without looking up documentation.

#### typeof - Type Detection Mechanism

**What it does:** Returns a string indicating the type of a value.

**How it works:** JavaScript internally tags values with type information. The `typeof` operator reads this tag.

```javascript
// Primitive types
typeof "hello";              // 'string'
typeof 42;                   // 'number'
typeof true;                 // 'boolean'
typeof undefined;            // 'undefined'
typeof Symbol();             // 'symbol'
typeof 123n;                 // 'bigint'

// Objects and functions
typeof {};                   // 'object'
typeof [];                   // 'object' (arrays are objects!)
typeof null;                 // 'object' (historical bug)
typeof function() {};        // 'function'

// Use for discovering methods
typeof "hello".slice;        // 'function' (it's a method)
typeof "hello".length;       // 'number' (it's a property)
```

**Practical discovery pattern:**

```javascript
// Filter an object to show only functions (methods)
Object.getOwnPropertyNames(String.prototype)
  .filter(prop => typeof String.prototype[prop] === 'function');
// Returns: ['charAt', 'concat', 'includes', 'indexOf', ...]
```

### Seeing the Prototype Chain in Action 🔬

#### Visualizing the Chain

```javascript
const arr = [1, 2, 3];

// Start at the instance
console.log('Instance:', arr);
console.log('Instance props:', Object.getOwnPropertyNames(arr));
// ['0', '1', '2', 'length']

// Move up to first prototype
const proto1 = Object.getPrototypeOf(arr);
console.log('Prototype 1:', proto1 === Array.prototype);  // true
console.log('Prototype 1 props:', Object.getOwnPropertyNames(proto1).slice(0, 5));
// ['constructor', 'concat', 'copyWithin', 'entries', 'every', ...]

// Move up to second prototype
const proto2 = Object.getPrototypeOf(proto1);
console.log('Prototype 2:', proto2 === Object.prototype);  // true
console.log('Prototype 2 props:', Object.getOwnPropertyNames(proto2));
// ['constructor', 'toString', 'valueOf', 'hasOwnProperty', ...]

// Move up to end of chain
const proto3 = Object.getPrototypeOf(proto2);
console.log('End of chain:', proto3);  // null
```

#### Creating Your Own Prototype Chain

Understanding through creation:

```javascript
// Create a "class" with prototype methods
function CustomArray() {
  this.items = [];
}

// Add methods to the prototype
CustomArray.prototype.add = function(item) {
  this.items.push(item);
};

CustomArray.prototype.size = function() {
  return this.items.length;
};

// Create an instance
const myArr = new CustomArray();

// The instance doesn't have these methods directly:
myArr.hasOwnProperty('add');     // false
myArr.hasOwnProperty('size');    // false

// But it can access them through the prototype chain:
'add' in myArr;                  // true
'size' in myArr;                 // true

// They exist on the prototype:
CustomArray.prototype.hasOwnProperty('add');   // true

// All instances share the same prototype:
const myArr2 = new CustomArray();
Object.getPrototypeOf(myArr) === Object.getPrototypeOf(myArr2);  // true
```

### Mental Model for Effective Exploration 💡

#### The Discovery Workflow

Now that you understand HOW these tools work, here's the mental model for using them:

**1. Start with the constructor's prototype** (the blueprint)

```javascript
Object.getOwnPropertyNames(Array.prototype)
// Shows ALL methods any array can use
```

**2. Filter to just methods** (exclude properties)

```javascript
Object.getOwnPropertyNames(Array.prototype)
  .filter(p => typeof Array.prototype[p] === 'function')
```

**3. Check method signatures** (how many parameters)

```javascript
Array.prototype.map.length        // 1
Array.prototype.reduce.length     // 1
```

**4. Verify availability** (especially for newer features)

```javascript
'includes' in Array.prototype     // Check if method exists
```

**5. Experiment with instances**

```javascript
[1,2,3].map(x => x * 2)          // Test behavior
```

#### Why Inspect Prototypes, Not Instances?

```javascript
// ❌ Inspecting an instance shows mostly noise:
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
Object.getOwnPropertyNames(arr);
// ['0', '1', '2', '3', '4', '5', '6', '7', '8', 'length']
// Just indices and length - not helpful for learning methods!

// ✅ Inspecting the prototype shows the methods:
Object.getOwnPropertyNames(Array.prototype);
// ['constructor', 'concat', 'map', 'filter', 'reduce', ...]
// All the methods you want to learn about!
```

#### The Complete Picture

```javascript
// Everything you need to explore any JavaScript type:

// 1. What methods exist?
Object.getOwnPropertyNames(String.prototype)
  .filter(p => typeof String.prototype[p] === 'function')

// 2. How many parameters?
String.prototype.slice.length  // 2

// 3. Can I use it?
'slice' in String.prototype    // true

// 4. Test it
"hello world".slice(0, 5)     // "hello"

// 5. Understand edge cases
"hello".slice(-3)             // "llo" (negative indices)
"hello".slice(10)             // "" (out of bounds)
```

