---
icon: square-up-right
---

# Understanding index.ts in TypeScript Projects

{% hint style="info" %}
`index.ts` is like a gateway file that makes importing stuff easier. Instead of importing from multiple files, you import everything from one place. It's super helpful in bigger projects to keep things organized!
{% endhint %}

### What is index.ts?

Think of `index.ts` as a reception desk for your code modules. It's where you decide what to show to visitors (other parts of your code) and what to keep private.

### Why Should I Care?&#x20;

* Makes imports cleaner
* Easier to reorganise code without breaking things
* Better control over what other developers can access

### Real-World Examples&#x20;

#### 1. Basic Setup

```typescript
// 📁 my-module/
//    ├── index.ts
//    ├── userTypes.ts
//    └── validationRules.ts

// userTypes.ts
export interface User {
  id: string;
  name: string;
}

// validationRules.ts
export const isValidEmail = (email: string) => {
  return email.includes('@');
}

// index.ts
export * from './userTypes';
export * from './validationRules';

// 🎯 Usage in another file
import { User, isValidEmail } from './my-module';  // So clean! 
```

#### 2. Feature Organisation&#x20;

```typescript
// 📁 features/
//    ├── auth/
//    │   ├── index.ts
//    │   ├── login.ts
//    │   └── signup.ts
//    └── profile/
//        ├── index.ts
//        ├── userInfo.ts
//        └── settings.ts

// auth/index.ts
export * from './login';
export * from './signup';

// 🎯 Usage
import { login, signup } from './features/auth';  // Nice and tidy!
```

#### 3. Selective Exports 🎯

```typescript
// 📁 utils/
//    ├── index.ts
//    └── helpers.ts

// helpers.ts
export const add = (a: number, b: number) => a + b;
export const subtract = (a: number, b: number) => a - b;
const internalHelper = () => console.log('internal');  // private!

// index.ts
export { add } from './helpers';  // Only exposing what we want!

// 🎯 Usage
import { add } from './utils';  // subtract isn't available!
```

### When to Use It? 🕒

* ✅ In feature modules
* ✅ Shared utilities
* ✅ Component libraries
* ✅ Any folder with multiple related files

### When to Skip It? ⏭️

* ❌ Single-file modules
* ❌ Super tiny projects
* ❌ When you need super-optimized performance

### Personal Tips 💭

1. Start using `index.ts` early in the project
2. Keep exports organised and documented
3. Use it to hide internal implementation details
4. Great for managing API boundaries

### Quick Reference 📚

```typescript
// Common export patterns:

// 1. Export everything
export * from './someFile';

// 2. Pick and choose
export { thing1, thing2 } from './someFile';

// 3. Rename while exporting
export { thing as newThing } from './someFile';

// 4. Mix local and re-exports
export * from './someFile';
export const VERSION = '1.0.0';
```

### Remember! 🧠

Think of `index.ts` as your module's API documentation. It shows what your module can do without revealing how it does it!
