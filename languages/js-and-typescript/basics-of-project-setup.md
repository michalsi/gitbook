---
icon: trowel-bricks
---

# Basics of project setup

## Initialise the project

```bash
npm init -y
```

is used when you’re starting a **Node.js / JavaScript project**. Let’s break it down:

***

#### 🔎 What it does

1. **`npm init`**
   * Initializes a new **npm project** in your current folder.
   * It creates a file called **`package.json`**, which describes your project (name, version, dependencies, scripts, etc.).
   * Normally, it asks you a series of questions (project name, author, license…).
2. **`-y` (or `--yes`)**
   * Skips all the interactive questions.
   * Accepts the default values for everything.
   * So instead of you typing answers, it just writes a `package.json` with defaults.

***

#### 📦 Resulting `package.json`

After running it, you’ll get a file like this in your folder:

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

***

#### ✅ Why it’s important

* `package.json` is the **manifest** of your project.
* It tracks:
  * **Dependencies** (packages you install with `npm install …`).
  * **Scripts** (shortcuts like `npm run start`).
  * **Metadata** (name, version, author).

Without it, you can’t properly manage dependencies or publish your project.

Add&#x20;

```json
  "type": "module",
```

## Set up libraries and tools

### 1️⃣ Dev dependencies

```bash
npm install --save-dev @types/jest @types/node jest ts-jest typescript supertest @types/supertest
```

These packages are installed with `--save-dev`, which means they’re **only needed for development and testing**, not in production.

* **`typescript`**\
  Lets you write TypeScript instead of plain JavaScript.
* **`jest`**\
  A popular testing framework for JavaScript/TypeScript. Runs your unit tests.
* **`ts-jest`**\
  A Jest transformer that allows Jest to understand TypeScript code without needing a separate build step.
* **`supertest`**\
  A library for testing HTTP endpoints (e.g., your API). You can write tests like `request(app).get("/api/matches")`.
* **`@types/jest`**\
  Type definitions for Jest, so TypeScript understands Jest functions like `describe`, `it`, `expect`.
* **`@types/node`**\
  Type definitions for Node.js APIs (e.g., `process`, `fs`), so TypeScript knows what Node built-ins look like.
* **`@types/supertest`**\
  Type definitions for Supertest, so your tests get autocomplete and type checking.

👉 Together, these let you **write and run strongly typed tests** against your Node/Express (or any backend) app.

***

### 2️⃣ Runtime dependencies

```bash
npm install axios dotenv
```

These packages are installed as **normal dependencies** because they’re needed when your app runs in production.

* **`axios`**\
  HTTP client for making requests (similar to `fetch`, but with better features like interceptors, timeouts, automatic JSON parsing).
*   **`dotenv`**\
    Loads environment variables from a `.env` file into `process.env`.\
    Useful for storing secrets/config like:

    ```
    DB_HOST=localhost
    DB_USER=myuser
    DB_PASS=mypassword
    ```

***

#### ✅ Summary

* **Dev deps** (`--save-dev`): Testing & TypeScript tooling → not needed in production.
* **Runtime deps**: Core libraries (`axios`, `dotenv`) → needed when the app runs.

## TypeScript + Jest Configuration

***

### 1️⃣ `tsconfig.json`

At the root of your project, create a **`tsconfig.json`** file (TypeScript configuration):

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "rootDir": "src",
    "outDir": "dist",
    "strict": true,
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "skipLibCheck": true
  },
  "include": ["src", "tests"]
}
```

* `src` → your TypeScript source code
* `tests` → your test files

<details>

<summary>Details of <code>tsconfig.json</code></summary>

This file tells the TypeScript compiler (**tsc**) how to treat your code.

### 🔎 `compilerOptions` fields

#### 1. **`target`**

* Defines the version of **JavaScript** to which your TypeScript is compiled.
* `"ES2020"` → uses modern features like optional chaining (`?.`), nullish coalescing (`??`), etc.
* If you want compatibility with older environments, you’d set `"ES5"` or `"ES6"`.

***

#### 2. **`module`**

* Specifies the module system TypeScript should use in the output.
* `"commonjs"` → standard for Node.js (uses `require`/`module.exports`).
* If targeting browsers with ES modules, you’d use `"esnext"` or `"ES2020"`.

***

#### 3. **`rootDir`**

* The folder containing your **TypeScript source code**.
* `"src"` means all your `.ts` files should live under `src/`.
* Ensures the output structure in `dist/` mirrors your source structure.

***

#### 4. **`outDir`**

* The folder where compiled **JavaScript files** will go.
* `"dist"` is a common convention.
* So `src/index.ts` → becomes `dist/index.js` after compilation.

***

#### 5. **`strict`**

* Enables all strict type-checking options (a bundle of several flags).
* Helps catch bugs early by being more type-safe.
* Includes things like:
  * `strictNullChecks` (null must be handled explicitly)
  * `strictFunctionTypes`
  * `noImplicitAny`

***

#### 6. **`esModuleInterop`**

* Makes it easier to work with **CommonJS** and **ES Modules** together.
*   Example: lets you do this cleanly:

    ```ts
    import express from "express"; // works even though express is CommonJS
    ```
*   Without it, you’d need:

    ```ts
    import * as express from "express";
    ```

***

#### 7. **`forceConsistentCasingInFileNames`**

* Ensures file imports use the correct casing.
* Example: If the file is `UserService.ts`, importing as `userservice.ts` will error (important on case-sensitive file systems like Linux/Mac).

***

#### 8. **`skipLibCheck`**

* Skips type checking of `.d.ts` files (definition files in `node_modules`).
* Speeds up compilation.
* Recommended in most projects, because library definitions are usually well-tested.

***

### 🔎 Top-level field outside `compilerOptions`

#### 9. **`include`**

* Tells TypeScript which folders/files to compile.
* Here we include:
  * `"src"` → your application source code
  * `"tests"` → your test files (so Jest + ts-jest can run them)

***

✅ In short:

* `target`, `module` → how JS is compiled
* `rootDir`, `outDir` → where code comes from and goes
* `strict`, `esModuleInterop` → safety + compatibility
* `forceConsistentCasingInFileNames`, `skipLibCheck` → stability/performance
* `include` → what files TypeScript should care about

</details>

***

### 2️⃣ `jest.config.js`

At the root, create a **`jest.config.js`** file:

```js
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  testMatch: ['**/tests/**/*.test.ts'],
  moduleFileExtensions: ['ts', 'js', 'json', 'node'],
};
```

This tells Jest to use **ts-jest** to compile `.ts` files on the fly.

***

### 3️⃣ `package.json` scripts

Update your `package.json` to include useful scripts:

```json
{
  "scripts": {
    "dev": "ts-node-dev --respawn --transpile-only src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js",
    "test": "jest --passWithNoTests",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage"
  }
}
```

#### What these do:

* **`npm run dev`** → Start your app in development mode (auto-reloads when code changes).
* **`npm run build`** → Compile TypeScript into plain JavaScript in `dist/`.
* **`npm start`** → Run the compiled app from `dist/`.
* **`npm test`** → Run all Jest tests once.
* **`npm run test:watch`** → Run tests in watch mode (rerun on file changes).
* **`npm run test:coverage`** → Run tests and show code coverage report.

***

### 4️⃣ Example test with **Supertest**

Inside a new folder `tests/`, create `app.test.ts`:

```ts
import request from "supertest";
import app from "../src/app"; // assuming you export your Express app

describe("GET /health", () => {
  it("should return healthy status", async () => {
    const res = await request(app).get("/health");
    expect(res.status).toBe(200);
    expect(res.body).toHaveProperty("status", "healthy");
  });
});
```

Run it with:

```bash
npm test
```

***

✅ Now you’ve got:

* TypeScript for coding
* Jest + ts-jest + Supertest for testing
* Scripts for dev, build, start, and test workflows



