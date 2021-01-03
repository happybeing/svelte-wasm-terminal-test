# Svelte WASI in Browser Example

This example demonstrates running Web Assembly (wasm) on WASI (the Web Assembly System Interface) in the browser using [WasmerJS]("https://github.com/wasmerio/wasmer-js).

It uses the wasm-terminal package from WasmerJS so you can type "cowsay something something" into a terminal window in the browser, and it will run the cowsay wasm app.

The project is based on [svelte-wasm-template](https://git.stlrust.org/j4ng5y/svelte-wasm-template.git) and so also includes some Rust/wasm code, but that is compiled for WASM (using wasm-pack) and not WASI at the moment.

## Prereqs

* Node.js
* Rust

## Get Started

```bash
git clone https://github.com/happybeing/svelte-wasm-terminal-test
cd svelte-wasm-terminal-test
yarn && yarn dev
```

Navigate to localhost:5000 and you should see the app running. 

The main code is in `src/App.svelte`.

The non-WASI Rust/WASM component is in `src/greeting/src`, save it, restart the dev server to recompile the WASM, and then reload your page to see your changes. NOTE: this is not compiled to WASI, but compiled using wasm-pack to WASM (using target wasm32-unknown-unknown).

If you're using VSCode, we recommend installing the offical Svelte extension as well as the offical Rust extension. If you are using other editors, your may need to install a plugin in order to get syntax highlighting and intellisense for both Svelte and Rust.

## Building and running in production mode

To create an optimized version of the app:

```bash
yarn build
```

You can run the newly built app with `yarn start`. This uses `sirv`, which is included in your `package.json`'s dependencies so that the app will work with you deploy to platforms like Heroku.

## Single-page app mode

By default, `sirv` will only respond to requests that match files in `public`. This is to maximize compatibility with static file servers, allowing you to deploy your app almost anywhere.

If you're building a single-page application (SPA) with multiple routes, `sirv` needs to be able to respond to requests for ANY path. You can make it so by editing the `start` command in `package.json`:

```bash
"start": "sirv public --single"
```
