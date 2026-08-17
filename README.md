> [!IMPORTANT]
> This repository does not include, distribute, or provide access to any game assets. It only contains the source code to build the recompilation. **You must own a legitimate copy of Condemned 2: Bloodshot** and extract the required assets from it.

> [!CAUTION]
> This project is a work-in-progress, and while it is in playable state, you may experience bugs and crashes!

# Condemned2Recomp

A static recompilation of **Condemned 2: Bloodshot (Xbox 360)** for Native PC built through [RexGlue-SDK](https://github.com/rexglue/rexglue-sdk).

<div align="center"><a href="https://www.youtube.com/watch?v=028_cJIhIjA"><img src="https://img.youtube.com/vi/028_cJIhIjA/maxresdefault.jpg" alt="Condemned 2 Recomp ( Preview for v0.1.1 - Performance Improvements" width="75%"></a><br>(Click thumbnail above to watch on Youtube)</div>

## Installation
1. Using [extract-xiso](https://github.com/XboxDev/extract-xiso) or [xdvdfs](https://github.com/antangelo/xdvdfs), extract all game files from your legally owned copy of **Condemned 2: Bloodshot**.
2. In the same folder as `condemned2recomp`, create a new folder named `Assets`.
3. Place extracted game files into the `Assets` folder.
4. **(Optional)** Configure your graphic settings by modifying `condemned2recomp.toml`.
5. Run `condemned2recomp` to start **Condemned 2: Bloodshot**.

## AMD / Mesa GPU Fix (Critical for AMD Users)

> [!WARNING]
> AMD GPU users on Linux (Mesa/RADV drivers) will experience a **blank screen during gameplay** due to a known Vulkan rendering bug with unclipped draw shaders. The menus render fine, but the game goes black the moment you load into gameplay.

To fix this, ensure your `condemned2recomp.toml` (in the same directory as the executable) contains:

```toml
execute_unclipped_draw_vs_on_cpu = true
```

> [!IMPORTANT]
> This setting **must be at the root level** of the TOML file — do NOT place it under a `[GPU]` or `[UI.Vulkan]` section header. The TOML parser flattens section names into the key (e.g., `[GPU].execute_unclipped_draw_vs_on_cpu` becomes `GPU_execute_unclipped_draw_vs_on_cpu`), which silently fails because no matching cvar exists.

### Example working config:
```toml
execute_unclipped_draw_vs_on_cpu = true
vulkan_allow_present_mode_immediate = false
```

### Optional: Wayland compositor workaround
If the screen still blanks after applying the TOML fix, force a controlled environment using gamescope:
```bash
gamescope -f -W 1920 -H 1080 -- ./condemned2recomp
```

## Controls

Keyboard & mouse support is still work-in-progress! Currently, it is recommended to use a gamepad to play.

| Action | Key |
| --- | --- |
| Movement | `W` `A` `S` `D` |
| Use | `E` |
| Swing Left / Aim | `LMB` |
| Swing Right / Shoot | `RMB` |
| Kick | `Space` |
| Reload | `R` |
| Sprint | `Shift` |
| Flashlight | `F` |
| Check | `H` |
| Pause | `Esc` |
| Settings | `F4` |
| Console | `` ` `` |

## Configure & Build

### Prerequisites

- [RexGlue SDK](https://github.com/rexglue/rexglue-sdk)
- [Visual Studio 2022 Community Edition](https://visualstudio.microsoft.com/vs/community) **with the Desktop development with C++ workload** (Windows)
- CMake 3.25+
- LLVM/Clang 20+
- Ninja

### Build
#### Download
```
git clone --recursive https://github.com/drDOOM69GAMING/Condemned2Recomp-Linux.git
```
#### Windows
```
cd Condemned2Recomp
cmake --preset win-amd64-release
cmake --build --preset win-amd64-release --target condemned2recomp_codegen
cmake --build --preset win-amd64-release
```
#### Linux **(Tested - CachyOS / AMD RX 6800)**
```
cd Condemned2Recomp
cmake --preset linux-amd64-release
cmake --build --preset linux-amd64-release --target condemned2recomp_codegen
cmake --build --preset linux-amd64-release
```

## Tested Configuration
- **OS:** Arch Linux (CachyOS)
- **GPU:** AMD Radeon RX 6800 (RADV NAVI21)
- **Driver:** Mesa (Vulkan 1.4.357)
- **Compiler:** Clang 22.1.8 with `-march=x86-64-v3`
- **Status:** Fully playable — menus and gameplay render correctly with the TOML fix applied

## Credits
* [RexGlue-SDK](https://github.com/rexglue/rexglue-sdk)
* [Ghidra](https://github.com/NationalSecurityAgency/ghidra)
* [RenderDoc](https://github.com/baldurk/renderdoc)
