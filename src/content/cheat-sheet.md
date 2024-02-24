---
title: Cheat Sheet
---

# Vite Cheat Sheet

> [aweasom vite](https://github.com/vitejs/awesome-vite)

vite use following bundlers:

- `esbuild`: in development mode
- `rollup`: for production

also vite use by default transpilers like `postCSS` for css and `babel` for js, see more down.

## create project

```sh
pnpm create vite (project-name) [--template (react|react-ts|vue|vue-ts)]
```

## add frameworks manually

to add framework manually or add more than one framework, install their vite plugin as dev dependencies

```js
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

// https://vitejs.dev/config/
export default defineConfig({
	plugins: [react()]
});
```

## built-in capabilities

- vite support ES modules and dynamic imports by default
- vite support global and css modules by default, use them with `rel` and as `imports`
- integrate with pre-processors easly by install them only, like `pnpm install -D pnpm`

## environment variables with vite

every vairable should start with `VITE_` as `VITE_MODE` and use them with `import.meta.env.MODE`

> all `*[.local]` files ignored by git

priority of variable if reproduced again is:

- before run command, `VITE_MODE=development node index.js`
- .env(.mode)[.local] (loaded in spesfic vite mode: `development` or `production`)
- .env[.local] (loaded in all cases)

## globl types

```ts
/// <reference types="vite/client" />
// export line so important
export {};
declare global {
   type ...;
   interface ...;
};
```

# vite config

## config define

there is two ways to define configs

```js
import { defineConfig, loadEnv } from 'vite'

// way one
export default defineConfig({})

// way two: command -> [build, serve, ....], mode -> [development, production], ssrBuild
export default defineConfig(({ command, mode, ssrBuild }) => {
   // if there await code here make it async
   // also we can load enviroment variables: mode -> which mode variables, cwd -> current working directory, '' -> vriables that which this string
   const env = loadEnv(mode, process.cwd(), '')
   // return configs, it can be conditional, like diffrent config for diffrent modes
   return {}
})
```

## config options

```js
import { defineConfig, loadEnv } from 'vite';

export default defineConfig({
	/* 
   set serve url in dev mode, 
   if you integrate with backend project make `base` as static files of backend framework
   also may change `base` for deployment reasons
   notice: used absolute assets like src="/vite" will not work with new base, src='vite' will work with any
   */
	base: '/static', // default is `/`
	envPrefix: 'APP_' /* default is VITE_ */,
	/* `direnv` mean `.env` files should be in directory `./direnv` */
	envDir: 'direnv' /* default is `.` */,
	build: {
		/* support newer js versions (not recommanded) */
		target: 'es2022',
		/* generate manifest.json in build dir, to easier mapping for pwa and backend integration */
		manifest: true,
		/* specify build directory, can be'static/' for bakends like django */
		outDir: 'dist/',
		/* 
   set entrypoints for components, can be single entrypoint for entire app
   default is index.html and index.js in frontend if this option doesnt set
   when make more than one entrypoint we enable chuncks
   it is for more than one html file for all project
   */
		rollupOptions: {
			input: {
				main: '/src/main.jsx',
				warning: '/src/warning.jsx',
				refrsh: 'refresh.js'
			}
		}
	},
	/* enable source map for css, scss, ... */
	css: {
		devSourcemap: true
	},
	server: {
		port: 8080,
		/* if port used and true it will not run on the next port */
		strictPort: true,
		/* enable proxy if connect to api from other domain */
		proxy: {
			'/web': {
				target: 'https://test.domain.com',
				changeOrigin: true,
				secure: false,
				rewrite: (path) => path
			}
		}
	},
	preview: {
		port: 8080,
		/* if port used and true it will not run on the next port */
		strictPort: true
	},
	/* disable clear screen with every reboot */
	clearScreen: false,
	/* `info`, `warn`, `error`, `silent` */
	logLevel: 'silent', // default is `info`
	/* 
      add to tsconfig:
      "baseUrl": ".",
      "paths": { "@/*": ["src/*"] }, 
   */
	resolve: { alias: { '@': '/src' } }
});
```

# plugins

vite have dozens of plugins -review aweasome vite- my prefer list is in [reactive template](github.com/wusaby-rush/reactive), but we may need write one. here is the method

other plugins to test (node, vite-plugin-dynamic-import, qrcode, vite-plugin-compression or compression2, vite-plugin-cem `custom elements`)

```js
// vite plugin
function twVariantGroups() {
  return {
    name: 'tw-variant-groups',
    transform(code) {
      const variantGroupsRegex = /([a-z\-0-9:]+:)\((.*?)\)/g;
      const variantGroupMatches = [...code.matchAll(variantGroupsRegex)];

      variantGroupMatches.forEach(([ matchStr, variants, classes ]) => {
        const parsedClasses = classes
          .split(' ')
          .map((cls) => variants + cls)
          .join(' ');
        code = code.replace(matchStr, parsedClasses);
      });
      return code;
    },
  };
}

// now in config file
plugins: [twVariantGroups(), ...]
```

## with unplugin

unplugin enable us to build universal plugins for build tools, see following example with nuxt

```js
import { createUnplugin } from 'unplugin';

const myplugin = createUnplugin(() => ({
	name: 'aweasomeviteplugin',
	transform: (code, id) => {
		return code.replace('Welcome to nuxt!', 'Welcome to nuxt!!');
	}
}));

export default defineNuxtConfig({
	vite: {
		plugins: [myplugin.vite()]
	}
});
```

- budget // nextjs
- dalil // app
- accounting // react app

// explain importance of name
// we can specify plugins to work at build or dev
كل بلجن تصدر هوكس عشان يستخدمه فيت في كل مرحلة من مراحل معالجة الملفات

`resolveId`: used when resolve imports paths -file import or module import- (default is search for this import in files or modules), first `resolveId` return string it will be the path for this file, and not use other plugins `resolveId`s

`load`: used when load imports contents (default is load content from file path), vite use plugins `load`s hooks to provide file content, first load return content it will not run other loaders

`transform`: used when process files contents, vite will run all plugins transformers,

https://rollupjs.org/plugin-development/
https://vitejs.dev/guide/api-plugin.html
