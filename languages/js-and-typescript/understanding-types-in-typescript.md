---
icon: kerning
---

# Understanding @types in TypeScript

The `@types/axios` package is part of the DefinitelyTyped repository and provides TypeScript type definitions for the axios library. Here's why we need both:

1. `axios`: The actual JavaScript library that provides HTTP client functionality
2. `@types/axios`: Contains TypeScript type declarations (`.d.ts` files) that help TypeScript understand:
   * Function signatures
   * Return types
   * Parameter types
   * Configuration object types
   * Response and error types

For example, without `@types/axios`, you'd get limited or no type information:

```typescript
// Without @types/axios, TypeScript doesn't know the types
const response = await axios.get('/api/users');
// response type is 'any'
// no autocomplete for response.data, response.status, etc.
```

With `@types/axios`, you get full TypeScript support:

```typescript
// With @types/axios, TypeScript knows exactly what types to expect
const response = await axios.get<User[]>('/api/users');
// response is typed as AxiosResponse<User[]>
response.data       // TypeScript knows this is User[]
response.status     // TypeScript knows this is number
response.headers    // TypeScript knows this is AxiosResponseHeaders
```

This pattern is common for JavaScript libraries that aren't written in TypeScript:

* The main package contains the actual implementation
* A corresponding `@types/*` package provides TypeScript definitions

Some examples:

```typescript
// Common packages needing separate type definitions
npm install react @types/react
npm install express @types/express
npm install jest @types/jest
```

Modern libraries that are written in TypeScript (like Angular or Deno) don't need separate `@types/*` packages because they include their type definitions in the main package.

Benefits of having type definitions:

1. Better IDE support with autocomplete
2. Catch errors at compile time
3. Better documentation through types
4. Safer refactoring
5. Better development experience

This is why in a TypeScript project, you'll often see both the main package and its corresponding `@types/*` package installed as dev dependencies.
