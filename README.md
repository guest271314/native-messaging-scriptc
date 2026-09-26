## scriptc Native Messaging host

> [scriptc](https://github.com/vercel-labs/scriptc)
>
> scriptc compiles TypeScript and JavaScript to typed IR, readable C, textual LLVM IR, native assembly and objects, native executables, and WebAssembly modules. It uses the TypeScript compiler for parsing and type checking. Source outputs require only Node. On macOS 15+ arm64, ordinary LLVM-tier executables use scriptc's bundled helper and precompiled runtime pack; clang is only the platform linker driver and does not compile program or runtime C.
>
> Static builds include a small native runtime, but no Node or JavaScript engine. Code that cannot compile statically is reported as a diagnostic. For npm packages and any-typed code, `--dynamic` embeds [quickjs-ng](https://github.com/quickjs-ng/quickjs) explicitly.
>
> scriptc is experimental and targets macOS, Linux, Windows, and WebAssembly via WASI Preview 1.
>
> #### [Build WebAssembly](https://github.com/vercel-labs/scriptc#build-webassembly)
> 
> WASI and other cross-target builds require Zig. Its bundled WASI libc produces a portable WASI Preview 1 module through the production LLVM backend:
>
> Install Zig and make sure the `zig` executable is available on your `PATH`. `SCRIPTC_CC=zigcc` is scriptc's selector for invoking Zig's `cc` subcommand; `zigcc` is not a standalone executable.

### Install

```shell
bun install --trust https://github.com/vercel-labs/scriptc
```

### Compile to native executable

```shell
SCRIPTC_CC=zigcc SCRIPTC_NO_CACHE=1 bun x scriptc build ./nm_scriptc_node_fs.ts --optimization=release --strip -o nm_scriptc_node_fs
```

### Compile to WASM WASI P1 target

```shell
SCRIPTC_CC=zigcc SCRIPTC_NO_CACHE=1 SCRIPTC_TARGET=wasm32-wasi bun x scriptc build ./nm_scriptc_node_fs.ts --optimization=release -o nm_scriptc_node_fs.wasm
```

### Installation and usage on Chrome and Chromium

1. Navigate to `chrome://extensions`.
2. Toggle `Developer mode`.
3. Click `Load unpacked`.
4. Select `native-messaging-script` folder.
5. Note the generated extension ID.
6. Open `nm_scriptc.json` in a text editor, set `"path"` to absolute path of `nm_scriptc_node_fs` (native executable), or `nm_scriptc.sh` (shellscript to execute `wasmtime nm_scriptc_node_fs.wasm`) and `chrome-extension://<ID>/` using ID from 5 in `"allowed_origins"` array; and make sure `wasmtime` is in `PATH` and `nm_scriptc.sh` is executable (when executing `nm_scriptc_node_fs.wasm` with a WASM runtime).
7. Copy the `nm_scriptc.json` file to Chrome or Chromium configuration folder, e.g., Chromium on Linux `~/.config/chromium/NativeMessagingHosts`; Chrome dev channel on Linux `~/.config/google-chrome-unstable/NativeMessagingHosts`.
8. To test click `service worker` link in panel of unpacked extension which is DevTools for `background.js` in MV3 `ServiceWorker`, observe echo'ed message from `scriptc` Native Messaging host. To disconnect run `port.disconnect()`.

The Native Messaging host echoes back the message passed. 

For differences between OS and browser implementations see [Chrome incompatibilities](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Chrome_incompatibilities#native_messaging).

## License
Do What the Fuck You Want to Public License [WTFPLv2](http://www.wtfpl.net/about/)
