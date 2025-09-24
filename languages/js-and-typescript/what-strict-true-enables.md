---
icon: garage
---

# What "strict": true enables

&#x20;`"strict": true` in **`tsconfig.json`** is actually a **bundle** of multiple compiler flags. It makes TypeScript very strict about types, which helps catch bugs early. Let’s break them all down.

When you turn this on, you’re automatically enabling these flags:

#### 1. **`noImplicitAny`**

* **Default off** → if TS can’t infer a type, it assumes `any`.
* **With strict** → you must explicitly type variables or function parameters.

```ts
function greet(name) {  // ❌ Error: Parameter 'name' implicitly has 'any'
  return "Hello " + name;
}
```

Fix:

```ts
function greet(name: string) {
  return "Hello " + name;
}
```

***

#### 2. **`strictNullChecks`**

* Forces you to handle `null` and `undefined` explicitly.
* Without this, `null` and `undefined` can sneak into any type.

```ts
let user: string = null; // ❌ Error with strictNullChecks
```

Fix:

```ts
let user: string | null = null; // ✅ Explicit union
```

***

#### 3. **`strictBindCallApply`**

* Checks type safety when using `bind`, `call`, or `apply`.

```ts
function add(a: number, b: number) {
  return a + b;
}

add.call(null, "1", "2"); // ❌ Error, must be numbers
```

***

#### 4. **`strictFunctionTypes`**

* Makes function type compatibility stricter.
* Ensures function parameters are checked **contravariantly** (a fancy word for "arguments must match exactly, not just be loosely compatible").

***

#### 5. **`strictPropertyInitialization`**

* In classes, all properties must be initialized in the constructor or explicitly marked optional.

```ts
class User {
  name: string; // ❌ Property 'name' has no initializer
}
```

Fix:

```ts
class User {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
}
```

or:

```ts
class User {
  name!: string; // ✅ definite assignment assertion
}
```

***

#### 6. **`alwaysStrict`**

* Ensures `"use strict";` is emitted in every file.
* Also makes parsing stricter (e.g., no accidental `with` statements).

***

#### 7. **`noImplicitThis`**

* Disallows using `this` when TypeScript can’t figure out its type.

```ts
function f() {
  console.log(this); // ❌ Error: 'this' has type 'any'
}
```

***

### ✅ Why use `"strict": true`?

* Forces you to be explicit → fewer bugs.
* Greatly improves autocomplete and type inference.
* Makes refactoring safer (compiler catches mistakes)

