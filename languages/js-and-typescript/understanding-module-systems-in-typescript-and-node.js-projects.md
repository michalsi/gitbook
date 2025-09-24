---
icon: container-storage
---

# Understanding Module Systems in TypeScript and Node.js Projects

Modern JavaScript development involves working with different module systems, primarily CommonJS (CJS) and ECMAScript Modules (ESM). When working with TypeScript, understanding these systems becomes even more important. Let's dive into how they work and how to configure your projects correctly.

### The Two Module Systems

#### CommonJS (CJS)

CommonJS is the traditional module system used by Node.js. It uses `require()` and `module.exports`:

```javascript
// Importing in CommonJS
const express = require('express');

// Exporting in CommonJS
module.exports = {
  someFunction,
  someVariable
};
```

#### ECMAScript Modules (ESM)

ESM is the modern standard for JavaScript, using `import` and `export` statements:

```javascript
// Importing in ESM
import express from 'express';

// Exporting in ESM
export const someFunction = () => {};
export default class MyClass {};
```

### The Package.json "type" Field

The `"type"` field in package.json is crucial for determining how Node.js interprets your JavaScript files:

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "type": "module"  // This tells Node.js to treat .js files as ESM
}
```

* When `"type": "module"` is set, `.js` files are treated as ESM
* Without it, `.js` files are treated as CommonJS
* You can mix both by using `.mjs` (for ESM) and `.cjs` (for CommonJS) extensions

### TypeScript Configuration

TypeScript adds another layer with its module configuration in `tsconfig.json`:

```json
{
  "compilerOptions": {
    "module": "nodenext",
    "moduleResolution": "nodenext"
  }
}
```

The `"module"` setting determines how TypeScript will transform your import/export statements:

* `"nodenext"`: Follows Node.js module resolution rules
* `"commonjs"`: Transforms to CommonJS format
* `"esnext"`: Keeps ESM syntax

### File Extensions in ESM

One important difference with ESM is that you need to include file extensions in imports:

```typescript
// ESM requires file extensions
import { helper } from './utils.js';  // Note the .js extension

// This won't work in ESM:
import { helper } from './utils';     // Missing extension
```

Even though you're writing `.ts` files, you need to use `.js` extensions in your imports because that's what the files will be after compilation.

### Practical Implications

#### 1. Jest Configuration

When using ESM, your Jest configuration needs adjustment:

```javascript
/** @type {import('jest').Config} */
export default {
  preset: 'ts-jest/presets/default-esm',
  moduleNameMapper: {
    '^(\\.{1,2}/.*)\\.js$': '$1'
  },
  extensionsToTreatAsEsm: ['.ts']
};
```

#### 2. TypeScript Path Aliases

If you're using path aliases, they need to include extensions:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*.js"]  // Note the .js extension
    }
  }
}
```

#### 3. Package Imports

Some packages might not support ESM or vice versa. Always check the package documentation for compatibility.

### Best Practices

1. **Choose Early**: Decide on your module system early in the project
2. **Be Consistent**: Stick to one system unless you have a compelling reason to mix
3. **Use TypeScript's Strict Checks**: Enable `"verbatimModuleSyntax": true` to catch module-related issues early
4. **Document Your Choice**: Add a note in your README about the module system used

### Common Issues and Solutions

#### 1. "Cannot use import statement outside a module"

Solution: Add `"type": "module"` to package.json

#### 2. "ERR\_REQUIRE\_ESM"

Solution: Use dynamic import or switch the importing file to ESM

#### 3. "Cannot find module ... or its corresponding type declarations"

Solution: Add proper file extensions in imports

### Conclusion

Understanding module systems is crucial for modern TypeScript development. While it might seem complex initially, having a clear understanding of how Node.js and TypeScript handle modules will save you many headaches down the road.

Remember:

* ESM is the future of JavaScript modules
* TypeScript needs proper configuration to work with your chosen module system
* File extensions matter in ESM
* Package compatibility should be checked when choosing a module system

By following these guidelines and understanding the underlying concepts, you can create more maintainable and efficient TypeScript applications.
