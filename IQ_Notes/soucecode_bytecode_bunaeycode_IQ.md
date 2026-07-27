# Source Code vs Byte Code vs Binary Code

## Example File: `intro.js` — `console.log("Hello, World!");`

| Aspect | Source Code | Byte Code | Binary Code (Machine Code) |
|--------|-------------|-----------|----------------------------|
| **What is it?** | Human-readable code written by a programmer in a high-level language | Intermediate representation — compiled from source but still platform-independent | Raw CPU instructions — 0s and 1s that the processor directly executes |
| **Who reads it?** | Humans (developers) | Virtual Machine / Interpreter (e.g., JVM, V8 engine) | CPU (hardware) |
| **Readability** | ✅ Easy to read and understand | ❌ Hard — looks like cryptic numbers/opcodes | ❌ Nearly impossible — just bits |
| **Portability** | ✅ Cross-platform (same code runs on any OS with the right runtime) | ✅ Cross-platform (needs a VM per platform) | ❌ Platform-specific (x86 vs ARM are totally different) |
| **Example** | `console.log("Hello, World!");` | `0x40, 0x1F, 0x03, 0x00, 0x00` (V8 Ignition bytecode for the above) — something like: `LdaGlobal "console"`, `GetProperty "log"`, `LdaSmi "Hello, World!"`, `Call` | `10101000 00000001 00000000 00000000 00000000 00000000 00000000 00000000` (x86-64 mov instructions that print to stdout) |
| **How it's produced** | Written by a developer in a text editor | Compiled from source (e.g., `javac`, V8's Ignition) | Assembled / JIT-compiled from byte code or assembly |
| **File extension** | `.js`, `.py`, `.java`, `.c`, etc. | `.class` (Java), `.pyc` (Python), internal V8 bytecode | `.exe`, `.out`, `.bin`, `.dll`, `.so` |
| **Speed** | Slowest (interpreted line-by-line) | Faster than source (pre-parsed) | Fastest (native CPU execution) |
| **Real-world flow (JS)** | You write: `console.log("Hello, World!");` | V8 engine compiles it to Ignition bytecode internally | TurboFan (JIT) compiles hot bytecode into native x86/ARM machine code |

## How It Flows Together

```
Source Code                          Byte Code                          Binary Code
(You write this)                     (V8 compiles this)                 (CPU runs this)
                                                                         
console.log("Hello, World!");  →     LdaGlobal "console"          →     mov rax, 0x7ff...  
                                      StkSlot, Star0                    call [rip+0x...]  
                                      LdaSmi "Hello, World!"            ...
                                      Call
```

## Key Takeaway

- **Source code** is **for you** — readable, writable, debuggable.
- **Byte code** is **for the runtime** — portable, compact, faster to interpret.
- **Binary code** is **for the hardware** — raw, fast, platform-dependent.

Most modern JS engines skip straight to **byte code** via a fast interpreter (V8's Ignition), then **JIT-compile hot paths to binary code** for speed (V8's TurboFan).
