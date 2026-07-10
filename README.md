# BNIL Emulator

A standalone Binary Ninja plugin that emulates Binary Ninja's Low Level Intermediate
Language (LLIL). It is structured like the [debugger](https://github.com/Vector35/debugger):
the engine builds into its own plugin (`emulatorcore`) against the public Binary Ninja
API, exposes a C ABI, and ships C++ (`emulatorapi`) and Python bindings.

> **Experimental.** This plugin is experimental and under active development; its API and
> behavior may change.

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

## Support status

Only **LLIL** emulation is supported today. **MLIL and HLIL emulation are planned** for the
future.

Emulating an unsupported instruction stops the emulator with an `Unimplemented` stop reason.

### Supported LLIL instructions

- **Constants:** `CONST`, `CONST_PTR`, `EXTERN_PTR`, `FLOAT_CONST`
- **Registers:** `REG`, `SET_REG`, `REG_SPLIT`, `SET_REG_SPLIT`, `LOW_PART`
- **Memory:** `LOAD`, `STORE`, `PUSH`, `POP`
- **Arithmetic:** `ADD`, `ADC`, `SUB`, `SBB`, `MUL`, `MULU_DP`, `MULS_DP`, `DIVU`, `DIVS`,
  `DIVU_DP`, `DIVS_DP`, `MODU`, `MODS`, `MODU_DP`, `MODS_DP`, `NEG`, `ADD_OVERFLOW`
- **Bitwise / shifts:** `AND`, `OR`, `XOR`, `NOT`, `LSL`, `LSR`, `ASR`, `ROL`, `ROR`, `RLC`,
  `RRC`, `SX`, `ZX`, `LOW_PART`, `TEST_BIT`, `BOOL_TO_INT`
- **Comparisons:** `CMP_E`, `CMP_NE`, `CMP_SLT`, `CMP_SLE`, `CMP_SGE`, `CMP_SGT`, `CMP_ULT`,
  `CMP_ULE`, `CMP_UGE`, `CMP_UGT`
- **Flags:** `FLAG`, `SET_FLAG`, `FLAG_BIT`, `FLAG_COND`, `FLAG_GROUP`
- **Control flow:** `JUMP`, `JUMP_TO`, `GOTO`, `IF`, `CALL`, `CALL_STACK_ADJUST`, `TAILCALL`,
  `RET`, `NORET`, `SYSCALL`
- **Other:** `NOP`, `BP`, `TRAP`, `INTRINSIC`, `UNDEF`, `UNIMPL`, `UNIMPL_MEM`

### Not yet supported

- **Floating point:** `FADD`, `FSUB`, `FMUL`, `FDIV`, `FSQRT`, `FABS`, `FNEG`, `FCMP_*`,
  `FLOAT_CONV`, `FLOAT_TO_INT`, `INT_TO_FLOAT`, `ROUND_TO_INT`, `CEIL`, `FLOOR`, `FTRUNC`
  (float *constants* are read, but float arithmetic is not evaluated)
- **Register stacks (x87/FPU-style):** `REG_STACK_REL`, `SET_REG_STACK_REL`, `REG_STACK_PUSH`,
  `REG_STACK_POP`, `REG_STACK_FREE_REG`, `REG_STACK_FREE_REL`
- **Bit operations:** `BSWAP`, `CLZ`, `CTZ`, `CLS`, `POPCNT`, `RBIT`, `ABS`, `MINS`, `MAXS`,
  `MINU`, `MAXU`
- **Other:** `ASSERT`, `FORCE_VER`, `CALL_PARAM`
