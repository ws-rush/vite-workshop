---
title: Unplugin
---

# vite config

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
