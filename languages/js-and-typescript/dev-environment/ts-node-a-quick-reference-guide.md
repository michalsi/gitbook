---
icon: square-share-nodes
---

# ts-node: A Quick Reference Guide

## What is ts-node? 🎯

**ts-node** is a TypeScript execution engine for Node.js that allows you to run `.ts` files directly without manual compilation.

**Simple formula:** `node` + TypeScript compiler = `ts-node`

It compiles TypeScript to JavaScript **in-memory** and executes it immediately—no intermediate `.js` files needed.

***

## The Problem It Solves 🔧

### Traditional TypeScript Workflow:

```bash
# Write TypeScript
vim solution.ts

# Compile to JavaScript
tsc solution.ts  # → creates solution.js

# Run with Node
node solution.js
```

**Pain points:**

* Manual compilation after every change
* Managing separate source and build directories
* Slower development iteration
* Need for file watchers and build tools

### With ts-node:

```bash
# One command: compile + run
ts-node solution.ts
```

No build artifacts. No intermediate steps. Just run.

***

## How It Works Internally ⚙️

ts-node hooks into Node's module system to intercept TypeScript files:

```
┌─────────────┐
│  script.ts  │  Your TypeScript file
└──────┬──────┘
       │
       ▼
┌─────────────────┐
│   ts-node       │  Intercepts .ts imports
└──────┬──────────┘
       │
       ▼
┌─────────────────┐
│ TypeScript      │  Compiles in-memory
│ Compiler (tsc)  │  (no files written)
└──────┬──────────┘
       │
       ▼
┌─────────────────┐
│ JavaScript      │  Executes immediately
│ (in memory)     │
└──────┬──────────┘
       │
       ▼
┌─────────────────┐
│   Node.js       │  Runs the code
└─────────────────┘
```

**Key mechanism:**

1. Registers a require hook via `require.extensions`
2. Intercepts `.ts` file loads
3. Compiles TypeScript → JavaScript in-memory
4. Caches compiled modules
5. Executes the result

***

## Usage Modes 📝

### 1. Direct Execution (CLI)

```bash
ts-node script.ts
ts-node src/utils/helper.ts
```

### 2. Require Hook (Programmatic)

```bash
node -r ts-node/register script.ts
```

**Common with test runners:**

```bash
# Mocha with TypeScript tests
mocha -r ts-node/register tests/**/*.ts

# Node with ts-node pre-loaded
node -r ts-node/register app.ts
```

### 3. Interactive REPL

```bash
$ ts-node
> const x: number = 42;
> console.log(x * 2);
84
> type User = { name: string; age: number };
> const user: User = { name: "Alice", age: 30 };
```

### 4. Programmatic API

```typescript
import { register } from 'ts-node';

register({
  transpileOnly: true,
  compilerOptions: {
    module: 'commonjs',
  },
});

// Now you can require .ts files
require('./some-typescript-file.ts');
```

***

## Configuration 🛠️

### Via tsconfig.json

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es2020",
    "strict": true
  },
  "ts-node": {
    "transpileOnly": true,
    "files": true,
    "compilerOptions": {
      "module": "commonjs"
    }
  }
}
```

### Via Environment Variables

```bash
# Specify config file location
TS_NODE_PROJECT=./tsconfig.custom.json ts-node script.ts

# Enable fast mode (skip type checking)
TS_NODE_TRANSPILE_ONLY=true ts-node script.ts

# Combine multiple settings
TS_NODE_PROJECT=./tsconfig.json TS_NODE_TRANSPILE_ONLY=true ts-node app.ts
```

### Key Options Explained

| Option            | Purpose                            | When to Use                                 |
| ----------------- | ---------------------------------- | ------------------------------------------- |
| `transpileOnly`   | Skip type checking, just transpile | Development, when IDE handles type checking |
| `files`           | Load files from tsconfig           | Need accurate project references            |
| `swc`             | Use SWC instead of tsc             | Need faster compilation                     |
| `compilerOptions` | Override tsconfig settings         | Environment-specific configs                |

***

## Common Use Cases ✅

### Perfect For:

**1. Development & Quick Experiments**

```bash
# Test a quick idea
ts-node explore.ts

# Run a utility script
ts-node scripts/seed-database.ts
```

**2. Testing Frameworks**

```bash
# Jest (via ts-jest)
jest

# Mocha with TypeScript
mocha -r ts-node/register 'tests/**/*.test.ts'

# Vitest (native TS support)
vitest
```

**3. Build & Development Scripts**

```typescript
// scripts/build.ts
import { execSync } from 'child_process';
import * as fs from 'fs';

console.log('🔨 Building project...');
execSync('tsc --build');
console.log('✅ Build complete!');
```

```bash
ts-node scripts/build.ts
```

**4. CLI Tools (Development)**

```typescript
// cli-tool.ts
import { program } from 'commander';

program
  .option('-d, --debug', 'output extra debugging')
  .parse(process.argv);

if (program.debug) console.log('Debug mode enabled');
```

**5. Rapid Prototyping**

```bash
# Quickly test an API integration
ts-node test-api.ts
```

### Not Ideal For: ❌

1. **Production deployments** → Use compiled JavaScript
2. **Performance-critical apps** → Compilation overhead adds latency
3. **Large codebases** → Slow startup times
4. **Serverless functions** → Cold start penalty
5. **Bundled applications** → Use webpack/vite/esbuild instead

***

## Performance Considerations 🚀

### The Speed Trade-off

```bash
# Fast: Pre-compiled JavaScript
node dist/app.js              # ~50ms startup

# Slower: On-the-fly compilation
ts-node src/app.ts            # ~500-1000ms startup
```

### Optimization: transpileOnly Mode

**Skip type checking** for faster execution:

```bash
# CLI flag
ts-node --transpile-only script.ts

# Environment variable
TS_NODE_TRANSPILE_ONLY=true ts-node script.ts

# tsconfig.json
{
  "ts-node": {
    "transpileOnly": true
  }
}
```

**When to use:**

* Your IDE/editor already does type checking
* Running tests (let CI handle type checking)
* Development iteration speed matters

### Using SWC for Speed

SWC is a Rust-based transpiler that's **10-20x faster** than tsc:

```bash
npm install -D @swc/core @swc/helpers
```

```json
{
  "ts-node": {
    "swc": true
  }
}
```

***

## Modern Alternatives 🆕

### tsx (Recommended for New Projects)

```bash
npm install -D tsx
tsx script.ts
```

**Benefits:**

* Uses esbuild (much faster)
* Better ESM support
* Simpler setup
* Watch mode built-in: `tsx watch script.ts`

### Bun

```bash
bun run script.ts
```

**Benefits:**

* Native TypeScript support
* Extremely fast
* All-in-one runtime (package manager + runtime + bundler)

### Comparison

| Tool          | Speed     | ESM Support | Use Case                                |
| ------------- | --------- | ----------- | --------------------------------------- |
| ts-node       | Moderate  | Good        | Established projects, testing           |
| tsx           | Fast      | Excellent   | Modern projects, development            |
| Bun           | Very Fast | Excellent   | Greenfield projects                     |
| ts-node + SWC | Fast      | Good        | Existing ts-node projects needing speed |

***

## Real-World Example: Test Setup 🧪

Here's how ts-node typically integrates with testing:

**package.json:**

```json
{
  "scripts": {
    "test": "mocha -r ts-node/register 'tests/**/*.test.ts'",
    "test:watch": "mocha -r ts-node/register 'tests/**/*.test.ts' --watch"
  },
  "devDependencies": {
    "mocha": "^10.0.0",
    "chai": "^4.3.0",
    "@types/mocha": "^10.0.0",
    "@types/chai": "^4.3.0",
    "ts-node": "^10.9.0",
    "typescript": "^5.0.0"
  }
}
```

**tests/math.test.ts:**

```typescript
import { expect } from 'chai';
import { add, multiply } from '../src/math';

describe('Math utilities', () => {
  it('should add numbers correctly', () => {
    expect(add(2, 3)).to.equal(5);
  });

  it('should multiply numbers correctly', () => {
    expect(multiply(4, 5)).to.equal(20);
  });
});
```

**Run tests:**

```bash
npm test
# ts-node compiles the TypeScript tests on-the-fly
# No separate build step needed
```

***

## Quick Reference Cheat Sheet 📋

### Installation

```bash
npm install -D ts-node typescript
# or
yarn add -D ts-node typescript
```

### Basic Commands

```bash
# Run a TypeScript file
ts-node script.ts

# With specific tsconfig
TS_NODE_PROJECT=tsconfig.dev.json ts-node script.ts

# Fast mode (no type checking)
ts-node --transpile-only script.ts

# Interactive REPL
ts-node

# With require hook
node -r ts-node/register script.ts
```

### Common package.json Scripts

```json
{
  "scripts": {
    "dev": "ts-node src/index.ts",
    "dev:watch": "nodemon --exec ts-node src/index.ts",
    "test": "mocha -r ts-node/register 'tests/**/*.ts'",
    "script": "ts-node scripts/build.ts"
  }
}
```

### Environment Variables

```bash
TS_NODE_PROJECT=./tsconfig.json       # Config file
TS_NODE_TRANSPILE_ONLY=true           # Skip type check
TS_NODE_COMPILER_OPTIONS='{"module":"commonjs"}'  # Override options
```

***

## When to Choose What 🤔

**Use ts-node when:**

* ✅ Developing and testing TypeScript projects
* ✅ Running build/utility scripts
* ✅ You have existing ts-node setup
* ✅ Need compatibility with mature tooling

**Use tsx when:**

* ✅ Starting a new project
* ✅ Need faster execution
* ✅ Want better ESM support
* ✅ Developing CLI tools

**Use Bun when:**

* ✅ Maximum speed is critical
* ✅ Starting from scratch
* ✅ Want all-in-one tooling
* ✅ Can adopt newer ecosystem

**Use pre-compiled JavaScript when:**

* ✅ Deploying to production
* ✅ Performance is critical
* ✅ Building for distribution
* ✅ Optimizing bundle size

***

## Troubleshooting Common Issues 🔍

### "Cannot find module" errors

```bash
# Ensure ts-node respects tsconfig paths
TS_NODE_PROJECT=./tsconfig.json ts-node script.ts

# Or enable files option
{
  "ts-node": {
    "files": true
  }
}
```

### ESM module issues

```json
{
  "ts-node": {
    "esm": true,
    "experimentalSpecifierResolution": "node"
  }
}
```

### Slow startup times

```bash
# Use transpileOnly
TS_NODE_TRANSPILE_ONLY=true ts-node script.ts

# Or switch to SWC
{
  "ts-node": {
    "swc": true
  }
}
```

***

## Summary 💡

**ts-node** bridges the gap between development convenience and TypeScript's compiled nature. It's essential for:

* Rapid development iteration
* Testing TypeScript code
* Running scripts and utilities
* Development tooling

Think of it as making TypeScript feel like an interpreted language while maintaining the benefits of static typing and compilation.

For **production**, always use compiled JavaScript. For **development**, ts-node (or modern alternatives like tsx) dramatically improves the developer experience.
