---
icon: ski-boot-ski
---

# Bootstrap TypeScript with Jest and Supertest

{% hint style="success" %}
Here's a one-liner that will initialise a Node.js project with TypeScript, Jest, ts-jest, and Supertest, plus generate `tsconfig.json` and Jest config automatically:

```bash
npm init -y && \
npm install --save-dev typescript jest ts-jest @types/jest @types/node \
supertest @types/supertest && \
npx tsc --init && \
npx ts-jest config:init
```
{% endhint %}

### What it does step by step:

1. `npm init -y` → initialises package.json with defaults
2. `npm install --save-dev ...` → installs all dev dependencies:
   * TypeScript and Node types
   * Jest, ts-jest, and Jest types
   * Supertest and its types
3. `npx tsc --init` → creates a default tsconfig.json
4. `npx ts-jest config:init` → creates jest.config.js preset for TypeScript

### Post-Setup Configuration

#### 1. Configure TypeScript (tsconfig.json)

Replace the default `tsconfig.json` with this more complete configuration:

```json
{
  "compilerOptions": {
    // File Layout
    "rootDir": "./src",
    "outDir": "./dist",

    // Environment Settings
    "module": "nodenext",
    "target": "esnext",
    "lib": ["esnext"],
    "types": ["node", "jest"],

    // Other Outputs
    "sourceMap": true,
    "declaration": true,

    // Type Checking
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "moduleResolution": "node"
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "**/*.test.ts", "**/*.spec.ts"]
}
```

#### 2. Configure Jest (jest.config.js)

Update `jest.config.js` with this enhanced configuration:

```javascript
/** @type {import("jest").Config} **/
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  roots: ['<rootDir>/src'],
  testMatch: ['**/__tests__/**/*.ts', '**/?(*.)+(spec|test).ts'],
  transform: {
    '^.+\\.ts$': 'ts-jest',
  },
  collectCoverageFrom: [
    'src/**/*.ts',
    '!src/**/*.d.ts',
  ],
  setupFilesAfterEnv: ['<rootDir>/src/setup.ts'],
  testTimeout: 10000
};
```

#### 3. Create Test Setup (src/setup.ts)

Create a setup file for global test configuration:

```typescript
// This file runs before each test file
// Add custom matchers or global test setup here

// Example: Increase default timeout for all tests
// jest.setTimeout(10000);

// Example: Add custom matchers
// expect.extend({
//   toBeWithinRange(received, floor, ceiling) {
//     const pass = received >= floor && received <= ceiling;
//     return {
//       message: () => 
//         pass 
//           ? `expected ${received} not to be within range ${floor} - ${ceiling}`
//           : `expected ${received} to be within range ${floor} - ${ceiling}`,
//       pass,
//     };
//   },
// });
```

#### 4. Update package.json Scripts

Add these comprehensive npm scripts to your package.json:

```json
{
  "scripts": {
    "dev": "ts-node-dev src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js",
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage",
    "clean": "rm -rf dist",
    "prebuild": "npm run clean"
  }
}
```

#### 5. Project Structure

Create this recommended folder structure:

```
your-project/
├── src/
│   ├── __tests__/        # Test files
│   ├── setup.ts          # Jest setup file
│   └── index.ts          # Main entry point
├── dist/                 # Compiled output
├── jest.config.js
├── tsconfig.json
├── package.json
└── README.md
```

#### Additional Tips

1. **VS Code Integration**:
   * Install the Jest VS Code extension for inline test running
   * Enable "Jest: Auto Run" in VS Code settings
2. **Optional Dependencies**:
   * For API testing: `npm install --save-dev supertest @types/supertest`
   * For hot reloading: `npm install --save-dev ts-node-dev`
3.  **Git Configuration**: Add this to your .gitignore:

    ```
    node_modules/
    dist/
    coverage/
    .env
    ```
4.  **Node Version**:

    * Recommended: Node.js 18+ for best compatibility with latest Jest
    * Add this to package.json:

    ```json
    {
      "engines": {
        "node": ">=18"
      }
    }
    ```

✅ Now your project is fully configured for:

* TypeScript development with modern ES features
* Jest testing with TypeScript support
* Code coverage reporting
* Development with hot reloading
* Clean production builds
