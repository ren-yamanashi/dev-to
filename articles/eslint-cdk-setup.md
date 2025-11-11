## Introduction

In this article, I'll show you how to introduce ESLint to your AWS CDK project.

> ℹ️ _**Note:** This article is targeted at CDK projects written in TypeScript._

---

## What is ESLint?

First, what exactly is ESLint?

[ESLint](https://eslint.org/) is a tool that analyzes JavaScript and TypeScript code to detect simple syntax errors and code that violates coding standards.

By using this tool, you can unify coding standards (writing style) among team members.

## 1. Installing ESLint

Let me walk you through the steps to introduce ESLint to your CDK project.

First, run the following command to install the necessary packages
(You'll install ESLint and the TypeScript plugin for ESLint)

```bash
# npm
npm install -D eslint typescript-eslint

# yarn
yarn add -D eslint typescript-eslint

# pnpm
pnpm install -D eslint typescript-eslint
```

## 2. Configuring package.json

Next, add a lint command to the `scripts` in `package.json` so that you can run ESLint commands from the terminal
(The `--fix` option applies automatic fixes.)

```diff
// package.json
{
  "scripts": {
+   "lint": "eslint .",
+   "lint:fix": "eslint . --fix"
  }
}
```

## 3. Creating the ESLint Configuration File

Next, create `eslint.config.mjs` in the project root:

```javascript
import eslint from "@eslint/js";
import { defineConfig } from "eslint/config";
import tsEslint from "typescript-eslint";

export default defineConfig(
  // Apply ESLint recommended settings
  eslint.configs.recommended,
  ...tsEslint.configs.recommended,
  {
    // Target TypeScript files under lib and bin directories
    files: ["lib/**/*.ts", "bin/*.ts"],
    languageOptions: {
      parserOptions: {
        projectService: true,
        project: "./tsconfig.json",
      },
    },
    plugins: {},
    rules: {},
  },
  // Specify files and directories to ignore
  {
    ignores: ["cdk.out", "node_modules", "*.js"],
  }
);
```

## 4. Installing Editor Extensions

Finally, install the extension to visualize code that violates ESLint rules in your editor (for VSCode).

[ESLint - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

And, add the following to `.vscode/settings.json`:

```diff
// .vscode/settings.json
{
+ "editor.defaultFormatter": "dbaeumer.vscode-eslint",
+ "eslint.useFlatConfig": true,
 ...
}
```

With this, the basic setup is complete

---

## Verification

Now that the basic setup is complete, let's verify it works with the following steps:

**1.** Include code that violates rules in a file under the lib directory, like this:

```ts
// lib/my-stack.ts
const a: any = ""
```

**2.** Run the following command to verify that the relevant code triggers an error:

```bash
# npm
npm run lint

# yarn
yarn lint

# pnpm
pnpm lint
```

### Troubleshooting

Here are solutions to common issues you might encounter when setting up ESLint.

#### Case1. ESLint Errors appear in terminal but not in editor

If you encounter this issue, reviewing the extension version or reloading (or restarting) the editor might be effective (since this is an editor-related issue).

#### Case2. No ESLint errors appear in terminal

If you encounter this issue, running `npm run lint --debug` to check if ESLint (or a specific ESLint rule) is functioning correctly may provide clues for resolution.

For more details, please refer to the following documentation:

https://eslint.org/docs/latest/use/configure/debug

---

## Customizing Rules

In the `eslint.config.mjs` above, I adopted the recommended rules.
If there are **rules you don't want to trigger errors** or **rules not included but you want to trigger errors**, you can solve this by customizing ESLint rules.

### Changing Rules

For example, if you don't want errors for "unused variables," modify the configuration file as follows:

```diff
// eslint.config.mjs
import eslint from "@eslint/js";
import { defineConfig } from "eslint/config";
import tsEslint from "typescript-eslint";

export default defineConfig(
    // omitted
    rules: {
+     "no-unused-vars": "off",
+     "@typescript-eslint/no-unused-vars": "off"
    },
  },
);
```

### Introducing Plugins

ESLint has a convenient plugin for CDK called `eslint-plugin-awscdk`, which allows you to apply the following rules:

[eslint-plugin-awscdk.dev](https://eslint-plugin-awscdk.dev)

#### Naming Convention Rules

| Description | Included in Recommended Rules |
|---------|------|
| Construct constructor properties should have 'scope, id' or 'scope, id, props' | ✅ |
| Don't add suffixes like "Construct" or "Stack" to Construct IDs | ✅ |
| Don't name Construct IDs after parent Construct names | ✅ |
| Don't use variables in Construct IDs | ✅ |
| Write Construct IDs in PascalCase | ✅ |
| Construct Props should be in the format `${ConstructName}Props` | |

#### Security Rules

| Description | Included in Recommended Rules |
|---------|------|
| Props (Interface) properties should have "readonly" | ✅ |
| Construct public variables should have "readonly" | ✅ |
| Don't specify Construct type for Props properties | ✅ |
| Don't specify Construct type for Construct public properties | ✅ |
| Always pass `this` to the second argument (`scope` property) of Construct constructor | ✅ |

#### Module Structure Rules

| Description | Included in Recommended Rules |
|---------|------|
| Don't call modules in private directories from outside | |

#### Documentation Rules

| Description | Included in Recommended Rules |
|---------|------|
| Write JSDoc for Props (Interface) properties and Construct public properties | |
| Write `@default` JSDoc for optional Props (Interface) properties | |

### Plugin Installation Steps

Here's how to install `eslint-plugin-awscdk`:

**1. Install the plugin**

```bash
# npm
npm install -D eslint-plugin-awscdk

# yarn
yarn add -D eslint-plugin-awscdk

# pnpm
pnpm install -D eslint-plugin-awscdk
```

**2. Update eslint.config.mjs**

```diff
// eslint.config.mjs
import eslint from "@eslint/js";
import { defineConfig } from "eslint/config";
import tsEslint from "typescript-eslint";
+import cdkPlugin from "eslint-plugin-awscdk";

export default defineConfig(
  // Apply ESLint recommended settings
  eslint.configs.recommended,
  ...tsEslint.configs.recommended,
  {
    // Target ts files under lib and bin directories
    files: ["lib/**/*.ts", "bin/*.ts"],
+   extends: [cdkPlugin.configs.recommended],
    // ...other settings
  },
);
```

---

## Conclusion

In this article, I introduced how to set up ESLint in CDK projects.

By introducing ESLint, you can expect to improve code quality and enhance team development efficiency.
In particular, by using `eslint-plugin-awscdk`, you can automatically check CDK-specific best practices.

Master ESLint and enjoy comfortable CDK development!!

## Documentation

[ESLint Documentation](https://eslint.org/docs/latest/)

[typescript-eslint Documentation](https://typescript-eslint.io/getting-started/)

[eslint-plugin-awscdk Documentation](https://eslint-plugin-awscdk.dev/)
