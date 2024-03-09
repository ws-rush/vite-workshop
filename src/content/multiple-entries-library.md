---
title: Multiple Entries Library
---

# Multiple Entries Library

```js
// vite.config,js
export default {
    build: {
        lib: {
            entry: [resolve(__dirname, 'src/index.js'), resolve(__dirname, 'src/log.js')]
            name: 'Pluk',
            // without this function, if name is `pluk`, they will exported as pluk, pluk2, ...
            filename: (format, name) => {
                if (format === 'es') return `${name}.js`
                return `${name}.${format}`
            }
        }
    }
}
```

```json
// package.json
{
	"main": "./dist/pluk.umd.cjs", // node require
	"module": "./dist/pluk.js", // ECMAScript import
	"exports": {
		// import { pluk } from 'pluk'
		".": {
			"require": "./dist/index.cjs", // node require
			"import": "./dist/index.js" // ECMAScript import
		},
		// import { log } from 'pluk/log'
		"./log": {
			"require": "./dist/log.cjs", // node require
			"import": "./dist/log.js" // ECMAScript import
		}
	}
}
```
