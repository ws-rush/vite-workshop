---
title: Usage with TypeScript
---

# Using TypeScript

Okay, but let's say you wanted to use TypeScript. That's probably a lot of set up, right?

Just change the file extension to `.ts`. That's it.

## Type Checking

Switching the file extension makes it compatible with Vite, but it won't perform type checks. To enable type checking, you can add the TypeScript compiler using the command `pnpm i -D typescript`. Afterward, include the following scripts in your configuration:

- `"tsc": "tsc --noEmit"`: This script checks TypeScript types without transpiling the code.
- `"tsc:watch": "tsc --noEmit --watch"`: This script watches for changes and continuously checks TypeScript types.

## Integrate Typescript with Vite

To transpile code using the TypeScript compiler (not just checking types), include the following script: `"build": "tsc && vite build"`. Additionally, ensure you have a `tsconfig.json` file to configure the TypeScript compiler. An example configuration could be:

```json
{
	"compilerOptions": { "rootDir": "./" }
}
```

For a more comprehensive integration that displays errors in the browser, consider using `vite-plugin-checker`. This plugin seamlessly integrates with ESLint and TypeScript, providing a smoother development experience with real-time error feedback.
