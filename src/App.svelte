<script>
export let greet;
import {onMount} from "svelte"

import WasmTerminal, { fetchCommandFromWAPM } from "@wasmer/wasm-terminal";
import { lowerI64Imports } from "@wasmer/wasm-transformer";

// Let's write handler for the fetchCommand property of the WasmTerminal Config.
const fetchCommandHandler = async ({args}) => {
  let commandName = args[0];
  // Let's return a "CallbackCommand" if our command matches a special name
  if (commandName === "callback-command") {
    const callbackCommand = async (options, wasmFs) => {
      return `Callback Command Working! Options: ${options}, fs: ${wasmFs}`;
    };
    return callbackCommand;
  }

  // Let's fetch a wasm Binary from WAPM for the command name.
  const wasmBinary = await fetchCommandFromWAPM({args});

  // lower i64 imports from Wasi Modules, so that most Wasi modules
  // Can run in a Javascript context.
  return await lowerI64Imports(wasmBinary);
};

onMount(() => {
	// Let's create our Wasm Terminal
	const wasmTerminal = new WasmTerminal({
		// Function that is run whenever a command is fetched
		fetchCommand: fetchCommandHandler
	});
	
	// Let's print out our initial message
	wasmTerminal.print("Hello World!");
	
	// Let's bind our Wasm terminal to it's container
	const containerElement = document.getElementById("#root");
	wasmTerminal.open(containerElement);
	wasmTerminal.fit();
	wasmTerminal.focus();
});
	
// Later, when we are done with the terminal, let's destroy it
// wasmTerminal.destroy();
</script>

<main>
<h1>Svelte WASI Example.</h1>

<p>This example demonstrates running Web Assembly (wasm) on WASI (the Web Assembly System Interface) in the browser using <a href="https://github.com/wasmerio/wasmer-js">WasmerJS</a>.</p>

<p>Type "cowsay something something" into the terminal.</p>

<p>This project also includes Rust/wasm code based on <a
href="https://git.stlrust.org/j4ng5y/svelte-wasm-template.git">svelte-wasm-template</a>,
which is used to get the following text from Rust: "{greet}". This Rust code is compiled for WASM (using wasm-pack) and not WASI at the moment.</p>

<div id="#root" name="#root"></div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>