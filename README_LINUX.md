# Condemned2Recomp - Linux Build Notes

## Build Fixes Applied

1. **SSSE3 flag** (`-mssse3`): Required by ReXGlue SDK's memory.cpp (`_mm_shuffle_epi8`)
2. **imgui include path**: Added `-I` flag for `thirdparty/imgui` (missing from SDK's target chain)
3. **VSync config**: Default `condemned2recomp.toml` forces FIFO present mode (no tearing)

## Build Commands

```bash
git clone --recursive https://github.com/psxrestore/Condemned2Recomp.git
cd Condemned2Recomp
mkdir -p assets && extract-xiso -x "Condemned2.iso" -d assets

cmake -S . -B out/build/linux-amd64-release \
  -G Ninja \
  -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_C_COMPILER=clang \
  -DCMAKE_CXX_COMPILER=clang++ \
  -DCMAKE_C_FLAGS="-mssse3" \
  -DCMAKE_CXX_FLAGS="-mssse3 -Ithirdparty/imgui" \
  -DREXSDK_DIR=/path/to/rexglue-sdk

cmake --build out/build/linux-amd64-release --target condemned2recomp_codegen
cmake --build out/build/linux-amd64-release
```

## Runtime

Copy the binary, `.so` libraries, and `Assets/` folder to a single directory. Use the included `condemned2recomp.toml` for vsync.
