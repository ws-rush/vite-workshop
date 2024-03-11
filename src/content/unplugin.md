---
title: Unplugin
---

# Unplugin

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
