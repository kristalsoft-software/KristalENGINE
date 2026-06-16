# KristalENGINE 2  =[TR](./README_tr.md)=

**KristalENGINE** is a standalone game engine powered by the **KristalBASIC** programming language — a modern, statically-typed language that combines the readability of C# and Swift with the raw performance of C. Write your game logic in KristalBASIC, and the engine compiles it into a native Windows executable (`.exe`) — no external runtime, no dependencies.

---

##  Features

- **KristalBASIC 2.0 Language** — Clean, expressive syntax inspired by C# and Swift. Static typing, classes, functions, and built-in game-oriented APIs right out of the box.
- **Full 2D & 3D Support** — Build anything from top-down RPGs and side-scrolling platformers to fully realized 3D games.
- **Visual Novel / Story Mode** — First-class support for text-based and narrative-driven games.
- **Physics Engine** — Built-in physics simulation for realistic movement and collision.
- **Audio System** — Integrated sound and music playback API.
- **Integrated IDE** — A purpose-built development environment with syntax highlighting, project management, and live compilation. Available from KristalBASIC 2.0 onwards.
- **Native Compilation** — Your project compiles directly to a standalone `.exe`. Ship your game without asking players to install anything.

---

##  What Can You Build?

| Genre | Supported |
|---|---|
| 2D Platformer / Sprite-based | ✅ |
| Top-down / RPG | ✅ |
| 3D Games | ✅ |
| Visual Novel / Text Adventure | ✅ |

---

##  Language Syntax

KristalBASIC 2.0 features a modern, statically-typed syntax. Here's a real-world example — a 3D first-person player input system:

```kristal
@OBJECT PlayerState.kbas

static class InputSystem {
    func int PlayerInput(PlayerState playerState) {
        decimal dt = FRAME_TIME();
        decimal mdx = MOUSE_DELTA_X() * -0.15;
        decimal mdy = MOUSE_DELTA_Y() * 0.15;

        playerState.yaw = playerState.yaw + mdx;
        playerState.pitch = playerState.pitch - mdy;

        if (playerState.pitch > 89.0) { playerState.pitch = 89.0; }
        if (playerState.pitch < -89.0) { playerState.pitch = -89.0; }

        playerState.dx = SIN(RAD(playerState.yaw)) * COS(RAD(playerState.pitch));
        playerState.dy = SIN(RAD(playerState.pitch));
        playerState.dz = COS(RAD(playerState.yaw)) * COS(RAD(playerState.pitch));

        decimal rx = SIN(RAD(playerState.yaw - 90.0));
        decimal rz = COS(RAD(playerState.yaw - 90.0));

        decimal moveSpeed = playerState.speed * dt;

        if (KEY_DOWN(87) == true) {
            playerState.x = playerState.x - (SIN(RAD(playerState.yaw)) * moveSpeed * -1);
            playerState.z = playerState.z - (COS(RAD(playerState.yaw)) * moveSpeed * -1);
        }
        if (KEY_DOWN(83) == true) {
            playerState.x = playerState.x + (SIN(RAD(playerState.yaw)) * moveSpeed * -1);
            playerState.z = playerState.z + (COS(RAD(playerState.yaw)) * moveSpeed * -1);
        }
        if (KEY_DOWN(65) == true) {
            playerState.x = playerState.x - (rx * moveSpeed);
            playerState.z = playerState.z - (rz * moveSpeed);
        }
        if (KEY_DOWN(68) == true) {
            playerState.x = playerState.x + (rx * moveSpeed);
            playerState.z = playerState.z + (rz * moveSpeed);
        }
        return 0;
    }
}
```

Key language features visible here: static classes, typed function signatures (`func int`), built-in engine functions (`FRAME_TIME()`, `KEY_DOWN()`, `SIN()`, `RAD()`), and object property access.

---

##  Getting Started

1. **Download** the latest release from [GitHub Releases](../../releases).
2. Run the installer and launch the **KristalENGINE IDE**.
3. Create a new project, write your first `.kbas` file, and hit **Build** to compile to `.exe`.

> **Requirements:** Windows 10 or later.

---

##  Distribution

KristalENGINE is **free to use**. The compiled `.exe` your project produces is yours — distribute it however you like.

> KristalENGINE is closed-source software. The compiler, IDE, and runtime internals are proprietary.

---

##  Roadmap

Have a feature request or found a bug? Open an [issue](../../issues) and let us know.

---

##  License

KristalENGINE is free to download and use. See [LICENSE](./LICENSE) for full terms.
