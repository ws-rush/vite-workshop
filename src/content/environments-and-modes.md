---
title: Environments and Modes
---

# Environments and Modes

Some things to cover:

1. **`import[dot]meta[dot]env`:** A special object for accessing environment variables.
2. **`.env` Files:** Used to define environment variables.
3. **Modes:** Development, production, etc., to manage multiple environment setups.
4. **TypeScript IntelliSense:** Autocompletion for custom variables.
5. **HTML Env Replacement:** Using `env` variables in HTML files.
6. **Security Considerations:** Warnings about sensitive data.

## Built-in Variables

- `MODE`: Running mode (development, production, etc.)
- `BASE_URL`: Base URL of the app
- `PROD`: Boolean, indicating if in production
- `DEV`: Boolean, indicating if in development
- `SSR`: Boolean, indicating if in server-side rendering mode
- **Production Replacement:** In production, these variables are statically replaced. Special consideration is needed for dynamic key access and Vue templates.

## `.env` Files

Vite uses `dotenv` and `dotenv-expand` libraries to load variables from `.env` files located in a specific directory.

- `.env`: Loaded always
- `.env.local`: Loaded always, ignored by git
- `.env.[mode]`: Loaded in a specified mode (development, production, etc.)
- `.env.[mode].local`: Loaded in a specified mode, ignored by git

There are some rules in terms of priority.

- Variables existing at the time of Vite execution (in shell) take the highest priority.
- Specific mode files override generic ones.

Vite is also looking out for your security.

- Only variables prefixed with `VITE_` are exposed to the client.
- change variable prefix with config `envPrefix: 'APP_'`
- Sensitive information should not be included. (Duh.)

## TypeScript IntelliSense

Custom `.d.ts` file can be created to provide TypeScript autocompletion for custom variables prefixed with `VITE_`.

Here is the one that I grabbed from [my own project](https://github.com/temporalio/ui/blob/main/src/env.d.ts).

```ts
/// <reference types="vite/client" />

type Vitest = import('vitest');

interface ImportMetaEnv {
	readonly VITE_TEMPORAL_UI_BUILD_TARGET: string;
	readonly VITE_API: string;
}

interface ImportMeta {
	readonly env: ImportMetaEnv;
	readonly vitest: Vitest;
}
```

## HTML Env Replacement

- Variables can be used in HTML files using `%ENV_NAME%` syntax, like `<pre>%VITE_API_URL%</pre>`.

## Modes

- `development` for dev server and `production` for build command, by default.
- Modes determine which `.env` files get loaded.
- Command-line flag `--mode` can be used to specify a mode, like `pnpm build -- --mode "staging"`.

## Security Notes

- `.env.*.local` files are local-only and should not be checked into version control.
- No sensitive data should be prefixed with `VITE_` as it will be exposed to the client.
