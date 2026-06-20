# KristalENGINE 2

=[TR](./README_tr.md)=

**KristalENGINE** is a standalone game engine powered by the **KristalBASIC** programming language — a modern, statically-typed language that combines the readability of C# and Swift with the raw performance of C. Write your game logic in KristalBASIC, and the engine compiles it into a native Windows executable (`.exe`) — no external runtime, no dependencies.

---

## Features

- **KristalBASIC 2.2 Language** — Clean, expressive syntax inspired by C# and Swift. Static typing, classes, functions, and built-in game-oriented APIs right out of the box.
- **Fully Independent Compiler Core** — The compiler (Lexer, Parser, CodeGen) is fully decoupled from any game engine dependency. KristalBASIC is now a general-purpose, modular, standalone programming language.
- **Modular Standard Library via @OBJECT** — External `.kbas` files are integrated into the build through the `@OBJECT` directive. Raylib bindings, Win32 API, and other modules are distributed as first-class standard library files (e.g. `lib/raylib.kbas`, `lib/win32/win32api.kbas`).
- **Direct C Integration** — Embed raw C/C++ source and header code directly inside `.kbas` files using isolated `external C` and `external H` blocks with triple-quote (`"""`) multi-line string literals.
- **Full 2D & 3D Support** — Build anything from top-down RPGs and side-scrolling platformers to fully realized 3D games.
- **Visual Novel / Story Mode** — First-class support for text-based and narrative-driven games.
- **Physics Engine** — Built-in physics simulation for realistic movement and collision.
- **Audio System** — Integrated sound and music playback API.
- **Native Windows API (Win32)** — Full access to the Windows operating system through `lib/win32/win32api.kbas`, requiring no external DLLs. Includes message boxes, system/RAM/CPU/monitor info, console I/O, file and directory operations, Windows Registry read/write, clipboard management, WAV playback, process and mutex management, and advanced window effects (borderless fullscreen, layered/transparent windows).
- **Integrated IDE** — A purpose-built development environment with syntax highlighting, project management, and live compilation.
- **Native Compilation** — Your project compiles directly to a standalone `.exe`. Ship your game without asking players to install anything.

---

## Language Features (2.2)

### New Primitive and Type Support

- **`const` keyword** — Declare compile-time constants. Constant assignments are translated naturally to their C equivalents.
- **Hexadecimal literals** — Write color and byte values in hex notation (e.g. `0xFFFFFF`). Macro name conflicts with C libraries are resolved through isolated compilation environments.
- **`void` type** — Functions that do not return a value can now be declared with `void`. The parser and code generator both recognize `void` and produce the correct C output.

### Object-Oriented Programming

- **Static classes** — `static class` declarations are compiled to a global C struct instance, so property access like `PlayerState.pitch = ...` maps directly to that global object.
- **`new` keyword** — Dynamic memory allocation via `kbc_alloc` is generated correctly for object instantiation.
- **`this` keyword** — Inside methods, `this` correctly references the hidden `THIS` pointer passed to the function.

### Memory Management

The Scope Manager has been overhauled. Stack-allocated C strings inside loop and conditional blocks (`while`, `if`) are no longer incorrectly freed during `kbc_scope_pop`, preventing fatal Heap Corruption errors (0xC0000374). The memory manager can now safely distinguish between pointers produced by `kbc_alloc` and those coming from external Win32 API calls. Strings from external calls are managed through `kbc_str_dup` and cleaned up automatically without leaks.

### String Comparison

`STR_EQ` is a new built-in function that bridges to C's `strcmp` for safe string comparison in conditional expressions (e.g. `if (STR_EQ(choice, "1"))`), eliminating pointer ambiguity.

### AST-Level Parameter Safety

Incorrect call sites — such as passing one argument to a function that declares zero parameters — are caught and reported by `kbc.exe` at the AST stage with a meaningful error message, rather than failing silently inside the C compiler.

### IDE / I/O Pipe Compatibility

When a program is launched from within the KristalENGINE IDE, standard I/O channels are redirected through pipes. `WriteConsole` and `ReadConsole` now detect this condition and fall back automatically to `WriteFile` and `ReadFile`, preventing infinite loops and ensuring output appears correctly in the IDE log panel. A `CON_OPEN` function is also available to forcibly detach from IDE pipes and open a fresh, independent console window for programs that require a terminal.

---

## What Can You Build?

| Genre | Supported |
|---|---|
| 2D Platformer / Sprite-based | Yes |
| Top-down / RPG | Yes |
| 3D Games | Yes |
| Visual Novel / Text Adventure | Yes |

---

## Language Syntax

KristalBASIC 2.2 features a modern, statically-typed syntax. Here is a real-world example — a 3D first-person player input system:

```kristal
@OBJECT PlayerState.kbas

static class InputSystem {
    func int PlayerInput(PlayerState playerState) {
        decimal dt = FRAME_TIME()
        decimal mdx = MOUSE_DELTA_X() * -0.15
        decimal mdy = MOUSE_DELTA_Y() * 0.15

        playerState.yaw = playerState.yaw + mdx
        playerState.pitch = playerState.pitch - mdy

        if (playerState.pitch > 89.0) { playerState.pitch = 89.0 }
        if (playerState.pitch < -89.0) { playerState.pitch = -89.0 }

        playerState.dx = SIN(RAD(playerState.yaw)) * COS(RAD(playerState.pitch))
        playerState.dy = SIN(RAD(playerState.pitch));
        playerState.dz = COS(RAD(playerState.yaw)) * COS(RAD(playerState.pitch))

        decimal rx = SIN(RAD(playerState.yaw - 90.0))
        decimal rz = COS(RAD(playerState.yaw - 90.0))

        decimal moveSpeed = playerState.speed * dt

        if (KEY_DOWN(87) == true) {
            playerState.x = playerState.x - (SIN(RAD(playerState.yaw)) * moveSpeed * -1)
            playerState.z = playerState.z - (COS(RAD(playerState.yaw)) * moveSpeed * -1)
        }
        if (KEY_DOWN(83) == true) {
            playerState.x = playerState.x + (SIN(RAD(playerState.yaw)) * moveSpeed * -1)
            playerState.z = playerState.z + (COS(RAD(playerState.yaw)) * moveSpeed * -1)
        }
        if (KEY_DOWN(65) == true) {
            playerState.x = playerState.x - (rx * moveSpeed)
            playerState.z = playerState.z - (rz * moveSpeed)
        }
        if (KEY_DOWN(68) == true) {
            playerState.x = playerState.x + (rx * moveSpeed)
            playerState.z = playerState.z + (rz * moveSpeed)
        }
        return 0
    }
}
```

Key language features visible here: static classes, typed function signatures (`func int`), built-in engine functions (`FRAME_TIME()`, `KEY_DOWN()`, `SIN()`, `RAD()`), and object property access.

---

## Getting Started

1. **Download** the latest release from [GitHub Releases](../../releases).
2. Run the installer and launch the **KristalENGINE IDE**.
3. Create a new project, write your first `.kbas` file, and hit **Build** to compile to `.exe`.

> **Requirements:** Windows 10 or later.

---

## Distribution

KristalENGINE is **free to use**. The compiled `.exe` your project produces is yours — distribute it however you like.

> KristalENGINE is closed-source software. The compiler, IDE, and runtime internals are proprietary.

---

## Roadmap

Have a feature request or found a bug? Open an [issue](../../issues) and let us know.

---

## License

KristalENGINE is free to download and use. See [LICENSE](./LICENSE.txt) for full terms.
