# BNIL Emulator

A standalone Binary Ninja plugin that emulates Binary Ninja's Low Level Intermediate
Language (LLIL). It is structured like the [debugger](https://github.com/Vector35/debugger):
the engine builds into its own plugin (`emulatorcore`) against the public Binary Ninja
API, exposes a C ABI, and ships C++ (`emulatorapi`) and Python bindings.

## Layout

- `core/` — the emulator engine (`ilemulator`, `llilemulator`) and plugin entry point,
  built as the `emulatorcore` plugin. Exposes the emulator C ABI (`api/ffi.h`).
- `api/` — C++ wrapper (`emulatorapi`, `BinaryNinja::LLILEmulator`) over the C ABI, plus
  Python bindings under `api/python/`.

## Building

```sh
export BN_API_PATH=/path/to/binaryninja-api
cmake -S . -B build           # -GNinja optional
cmake --build build
```

The resulting `emulatorcore` plugin is written to `build/out/plugins/`.

## Status

Extraction from Binary Ninja core in progress — see the repository history. The engine is
being ported from the internal `BinaryNinjaCore` API onto the public `BinaryNinja` API.
