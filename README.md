Example repo for issue https://github.com/pnpm/pnpm/issues/4382

## Reproduce

```
pnpm install
pnpm run rollup
```

This will cause error:

```
[!] SyntaxError: Unexpected token 'export'
.../node_modules/.pnpm/@babel+types@7.17.0/node_modules/@babel/types/lib/ast-types/generated/index.js:1
export { createFileSessionStorage, unstable_createFileUploadHandler, unstable_createMemoryUploadHandler, unstable_parseMultipartFormData } from "@remix-run/node";
^^^^^^

SyntaxError: Unexpected token 'export'
    at Object.compileFunction (node:vm:352:18)
    at wrapSafe (node:internal/modules/cjs/loader:1032:15)
    at Module._compile (node:internal/modules/cjs/loader:1067:27)
    at Module._extensions..js (node:internal/modules/cjs/loader:1155:10)
    at Object.require.extensions.<computed> [as .js] (.../node_modules/.pnpm/rollup@2.68.0/node_modules/rollup/dist/shared/loadConfigFile.js:621:13)
    at Module.load (node:internal/modules/cjs/loader:981:32)
    at Function.Module._load (node:internal/modules/cjs/loader:822:12)
    at Module.require (node:internal/modules/cjs/loader:1005:19)
    at require (node:internal/modules/cjs/helpers:102:18)
    at Object.<anonymous> (.../node_modules/.pnpm/@babel+types@7.17.0/node_modules/@babel/types/lib/index.js:629:19)
```

because Remix in postinstall script `remix setup node` writes TypeScript types to file `node_modules/remix/index.d.ts`, and changes automatically (because of hardlink) goes to `node_modules/.pnpm/@babel+types@7.17.0/node_modules/@babel/types/lib/ast-types/generated/index.js` as well (they are actually the same file).

```
$ ls -li node_modules/remix/index.d.ts node_modules/.pnpm/@babel+types@7.17.0/node_modules/@babel/types/lib/ast-types/generated/index.js

450024896 .rw-r--r-- vic staff 1.6 KB Wed Feb 23 21:51:08 2022 node_modules/remix/index.d.ts
450024896 .rw-r--r-- vic staff 1.6 KB Wed Feb 23 21:51:08 2022 node_modules/.pnpm/@babel+types@7.17.0/node_modules/@babel/types/lib/ast-types/generated/index.js
```
