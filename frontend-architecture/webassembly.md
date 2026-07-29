# WebAssembly in the Browser

WebAssembly (Wasm) is a binary instruction format that provides near-native performance in the browser. It is not a replacement for JavaScript but a complement for performance-critical workloads.

---

## What WebAssembly Enables

- **Performance-critical computation**: Image/video processing, 3D rendering, physics simulations
- **Porting existing codebases**: C/C++, Rust, Go, Zig compiled to run in the browser
- **Cryptography and compression**: Fast hashing, encoding, compression/decompression
- **Game engines**: Unity, Unreal Engine compiled to Wasm for browser-based games
- **Desktop app ports**: Figma (C++ compiled to Wasm), Photoshop (via Wasm)

---

## How It Works

```
Source Code (C/Rust/Go) → Compiler (Emscripten/wasm-pack) → .wasm binary → Browser
```

Wasm runs in a sandboxed virtual machine with the following characteristics:

- **Linear memory**: Flat array of bytes allocated by the module
- **No garbage collection**: Manual memory management (Rust's ownership model or explicit free)
- **No DOM access**: Wasm cannot manipulate the DOM directly — must call JavaScript via imports
- **Streaming compilation**: Browser compiles Wasm as it downloads (`WebAssembly.instantiateStreaming`)

---

## Toolchains

### Emscripten (C/C++ → Wasm)

The most mature toolchain. Compiles C/C++ codebases (e.g., Unity, FFmpeg, SQLite) to Wasm with optional JavaScript glue code.

```bash
emcc main.c -o output.js -s WASM=1 -s EXPORTED_FUNCTIONS="[_main]"
```

### wasm-pack (Rust → Wasm)

The official build tool for compiling Rust to Wasm, targeting the `wasm32-unknown-unknown` target.

```bash
wasm-pack build --target web
```

### Go → Wasm

Go has built-in Wasm support (`GOOS=js GOARCH=wasm`). Produces a `.wasm` binary along with a JavaScript runtime.

```bash
GOOS=js GOARCH=wasm go build -o main.wasm
```

---

## Wasm in Practice

```js
// Loading and instantiating a Wasm module
const response = await fetch('module.wasm');
const bytes = await response.arrayBuffer();
const { instance } = await WebAssembly.instantiate(bytes, {
  imports: { log: (msg) => console.log(msg) },
});
const result = instance.exports.compute(data);
```

---

## Limitations

- **No DOM access**: Must go through JavaScript for any DOM interaction
- **No garbage collection**: Manual memory management (GC proposal is in development)
- **Startup cost**: Parsing and compiling Wasm binaries (though streaming compilation helps)
- **Large binary size**: Wasm is not smaller than JS for typical UI code
- **Limited tooling**: Debugging and profiling tools are less mature than for JavaScript

---

[⬅️ Back to Frontend Architecture](./README.md)
