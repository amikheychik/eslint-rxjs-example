# Readme

`index.js` is parsed as a ES module, running it as `node index.js` fails:

```
node index.js
node:internal/modules/esm/resolve:296
  return new ERR_PACKAGE_PATH_NOT_EXPORTED(
         ^

Error [ERR_PACKAGE_PATH_NOT_EXPORTED]: No "exports" main defined in /Users/amikheychik/Code/Perfective/typescript/eslint-rxjs/node_modules/@smarttools/eslint-plugin-rxjs/package.json imported from /Users/amikheychik/Code/Perfective/typescript/eslint-rxjs/index.js
    at exportsNotFound (node:internal/modules/esm/resolve:296:10)
    at packageExportsResolve (node:internal/modules/esm/resolve:586:13)
    at packageResolve (node:internal/modules/esm/resolve:823:14)
    at moduleResolve (node:internal/modules/esm/resolve:907:18)
    at defaultResolve (node:internal/modules/esm/resolve:1037:11)
    at ModuleLoader.defaultResolve (node:internal/modules/esm/loader:650:12)
    at #cachedDefaultResolve (node:internal/modules/esm/loader:599:25)
    at ModuleLoader.resolve (node:internal/modules/esm/loader:582:38)
    at ModuleLoader.getModuleJobForImport (node:internal/modules/esm/loader:241:38)
    at ModuleJob._link (node:internal/modules/esm/module_job:132:49) {
  code: 'ERR_PACKAGE_PATH_NOT_EXPORTED'
}

Node.js v22.11.0
```

`index.cjs` is parsed as a CommonJS module, running it as `node index.cjs` succeeds:

```
node index.cjs
{
  meta: { name: '@smarttools/eslint-plugin-rxjs', version: '1.0.0' },
  rules: {
    'ban-observables': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'ban-operators': { create: [Function: create], defaultOptions: [], meta: [Object] },
    finnish: { create: [Function: create], defaultOptions: [], meta: [Object] },
    just: { create: [Function: create], defaultOptions: [], meta: [Object] },
    macro: { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-async-subscribe': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-compat': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-connectable': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-create': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-cyclic-action': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-exposed-subjects': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-finnish': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-ignored-error': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-ignored-notifier': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-ignored-observable': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-ignored-replay-buffer': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-ignored-subscribe': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-ignored-subscription': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-ignored-takewhile-value': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-implicit-any-catch': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-index': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-internal': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-nested-subscribe': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-redundant-notify': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-sharereplay': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-subclass': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-subject-unsubscribe': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-subject-value': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-subscribe-handlers': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-tap': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-topromise': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-unbound-methods': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-unsafe-catch': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-unsafe-first': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-unsafe-subject-next': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-unsafe-switchmap': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'no-unsafe-takeuntil': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'prefer-observer': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'suffix-subjects': { create: [Function: create], defaultOptions: [], meta: [Object] },
    'throw-error': { create: [Function: create], defaultOptions: [], meta: [Object] }
  },
  configs: {
    recommended: {
      name: '@smarttools/rxjs/recommended',
      plugins: [Object],
      rules: [Object]
    },
    'recommended-legacy': {
      plugins: [Array],
      name: '@smarttools/rxjs/recommended-legacy',
      rules: [Object]
    }
  }
}
```

To resolve the problem update `/node_modules/@smarttools/eslint-plugin-rxjs/package.json` to have exports as:

```
"exports": {
    ".": {
      "require": "./index.cjs",
      "types": "./index.d.ts",
      "import": "./index.cjs"
    }
  }
```

After you copy the fixed `package.json`:

```
cp ./eslint-rxjs-package.json ./node_modules/@smarttools/eslint-plugin-rxjs/package.json
```

`node index.js` starts working.