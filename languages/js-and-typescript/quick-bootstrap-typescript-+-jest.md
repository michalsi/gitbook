---
icon: ski-boot-ski
---

# Quick bootstrap TypeScript + Jest

{% hint style="success" %}
Here’s a **one-liner** that will initialise a Node.js project with **TypeScript, Jest, ts-jest, and Supertest**, plus generate `tsconfig.json` and Jest config automatically:

```bash
npm init -y && \
npm install --save-dev typescript jest ts-jest @types/jest @types/node \
supertest @types/supertest && \
npx tsc --init && \
npx ts-jest config:init
```
{% endhint %}

***

#### What it does step by step:

1. `npm init -y` → initializes `package.json` with defaults
2. `npm install --save-dev ...` → installs all dev dependencies:
   * TypeScript, Jest, ts-jest, Node/Jest types, Supertest, Supertest types
3. `npx tsc --init` → creates a default `tsconfig.json`
4. `npx ts-jest config:init` → creates `jest.config.js` preset for TypeScript

***

After running this, you’ll just need to:

* Create a `src/` folder for your TS files
* Optionally create a `tests/` folder for tests
* Add npm scripts to `package.json`:

```json
"scripts": {
  "dev": "ts-node-dev src/index.ts",
  "build": "tsc",
  "start": "node dist/index.js",
  "test": "jest",
  "test:watch": "jest --watch"
}
```

✅ That’s it — your project is ready to write **TypeScript code + run tests with Jest + Supertest**.
