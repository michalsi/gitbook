---
icon: burrito
---

# Why Do We Wrap Function Calls in Arrow Functions with Jest’s .toThrow()?

If you’ve ever written tests in [Jest](https://jestjs.io/) for JavaScript or TypeScript, you’ve probably seen this pattern:

```javascript
expect(() => compileAndroidCode()).toThrow();
```

But why do we need to wrap `compileAndroidCode()` in an arrow function? Why not just write `expect(compileAndroidCode()).toThrow()`? Let’s dig into the details!

***

### The Core of the Issue: When Does the Function Run?

The key difference is **when** the function gets executed:

* `expect(compileAndroidCode())`: Executes `compileAndroidCode()` **immediately**—before Jest even gets a chance to apply `toThrow()`. If an error is thrown, it escapes before Jest can catch or check it.
* `expect(() => compileAndroidCode())`: Passes a **function reference** to Jest, so Jest can execute it **inside** a try/catch block, just when it’s ready to check for errors.

***

### An Analogy

Think of it like this:

* `expect(compileAndroidCode())` is like throwing a ball before anyone is ready to catch it.
* `expect(() => compileAndroidCode())` is like handing someone the ball and saying, “Throw this when you’re ready.”

***

### What Does Jest Expect?

The `.toThrow()` matcher expects a **function** as its argument:

* If you pass the result of a function call that throws, the error is thrown too early.
* If you pass a reference (or a wrapper, like an arrow function), Jest gets control and can properly test for thrown errors.

***

### What if My Function Has No Arguments?

You can also just pass the function reference directly:

```javascript
expect(compileAndroidCode).toThrow();
```

But if your function takes arguments, you must use an arrow function:

```javascript
expect(() => someFunction(arg1, arg2)).toThrow();
```

***

### Quick Reference Table

| Syntax                                   | Will it work? | Why?                                           |
| ---------------------------------------- | :-----------: | ---------------------------------------------- |
| `expect(compileAndroidCode())`           |       ❌       | Executes too early, Jest can’t catch the error |
| `expect(compileAndroidCode)`             |  ✅ (no args)  | Passes reference, Jest controls execution      |
| `expect(() => compileAndroidCode())`     |       ✅       | Passes a wrapper, Jest controls execution      |
| `expect(() => someFunction(arg1, arg2))` |       ✅       | Needed for functions with arguments            |

***

### The TL;DR

> **Always pass a function (or a function wrapper) to `expect(...).toThrow()` so Jest can catch and assert the error properly.**

If you pass the result of a function that throws, the error escapes before Jest can test for it!
