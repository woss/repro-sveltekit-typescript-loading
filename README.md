# Repro for wrong typescript loading

Reproduction steps:

```sh
pnpm install
```

Modify the `ts.js` file in the `ui/app/node_modules?@sveltejs/kit/src/core/sync/ts.js` to see errors and version:

```js
/** @type {import('typescript')} */
// @ts-ignore
export let ts = undefined;
try {
  ts = (await import("typescript")).default;
  console.log("ts version", ts.version);
} catch (error) {
  console.error(error);
}
```

If the version is correctly loaded ( which is not ) you should not see error and version `5.3.3` but you will see that it loads `4.5.2`.

Now we need to edit the `write_types/index.js` in the same directory as `ts.js`. Find the `catch` at the end of the file and add the logging like this:

```js
catch (error) {
    console.error(error);
    return null;
}
```

Now you are ready to see the error:

```sh
cd ui/app
pnpm dev

#output

ts version 4.5.2
TypeError: ts.getModifiers is not a function
    at file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:691:7
    at visitNodes (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:29880:30)
    at Object.forEachChild (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:30124:24)
    at NodeObject.forEachChild (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:159412:23)
    at tweak_types (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:673:7)
    at createProxy (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:565:16)
    at ensureProxies (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:555:23)
    at process_node (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:415:2)
    at update_types (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:266:7)
    at write_all_types (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:127:4)
TypeError: ts.getModifiers is not a function
    at file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:691:7
    at visitNodes (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:29880:30)
    at Object.forEachChild (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:30124:24)
    at NodeObject.forEachChild (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:159412:23)
    at tweak_types (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:673:7)
    at createProxy (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:565:16)
    at ensureProxies (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:555:23)
    at file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:308:5
    at Array.forEach (<anonymous>)
    at update_types (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:297:29)
TypeError: ts.getModifiers is not a function
    at file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:691:7
    at visitNodes (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:29880:30)
    at Object.forEachChild (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:30124:24)
    at NodeObject.forEachChild (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:159412:23)
    at tweak_types (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:673:7)
    at createProxy (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:565:16)
    at ensureProxies (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:555:23)
    at file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:308:5
    at Array.forEach (<anonymous>)
    at update_types (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:297:29)
TypeError: ts.getModifiers is not a function
    at file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:691:7
    at visitNodes (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:29880:30)
    at Object.forEachChild (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:30124:24)
    at NodeObject.forEachChild (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:159412:23)
    at tweak_types (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:673:7)
    at createProxy (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:565:16)
    at ensureProxies (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:551:20)
    at file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:308:5
    at Array.forEach (<anonymous>)
    at update_types (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:297:29)
TypeError: ts.getModifiers is not a function
    at file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:691:7
    at visitNodes (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:29880:30)
    at Object.forEachChild (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:30124:24)
    at NodeObject.forEachChild (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:159412:23)
    at tweak_types (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:673:7)
    at createProxy (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:565:16)
    at ensureProxies (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:555:23)
    at file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:308:5
    at Array.forEach (<anonymous>)
    at update_types (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:297:29)
TypeError: ts.getModifiers is not a function
    at file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:691:7
    at visitNodes (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:29880:30)
    at Object.forEachChild (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:30124:24)
    at NodeObject.forEachChild (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:159412:23)
    at tweak_types (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:673:7)
    at createProxy (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:565:16)
    at ensureProxies (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:555:23)
    at process_node (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:415:2)
    at update_types (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:266:7)
    at write_all_types (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:127:4)
TypeError: ts.getModifiers is not a function
    at file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:691:7
    at visitNodes (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:29880:30)
    at Object.forEachChild (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:30124:24)
    at NodeObject.forEachChild (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:159412:23)
    at tweak_types (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:673:7)
    at createProxy (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:565:16)
    at ensureProxies (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:551:20)
    at process_node (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:415:2)
    at update_types (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:266:7)
    at write_all_types (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:127:4)
TypeError: ts.getModifiers is not a function
    at file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:691:7
    at visitNodes (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:29880:30)
    at Object.forEachChild (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:30124:24)
    at NodeObject.forEachChild (//HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/typescript@4.5.2/node_modules/typescript/lib/typescript.js:159412:23)
    at tweak_types (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:673:7)
    at createProxy (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:565:16)
    at ensureProxies (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:555:23)
    at process_node (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:415:2)
    at update_types (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:266:7)
    at write_all_types (file:////HOME-ROOT/repro-sveltekit-typescript-loading/node_modules/.pnpm/@sveltejs+kit@2.5.2_@sveltejs+vite-plugin-svelte@3.0.2_svelte@4.2.12_vite@5.1.4/node_modules/@sveltejs/kit/src/core/sync/write_types/index.js:127:4)

```
