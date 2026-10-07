# luajit-cmake

A flexible cmake builder for LuaJIT.

## Getting the Source

LuaJIT is bundled as a git submodule in `LuaJIT/`. Clone recursively, or
initialize the submodule in an existing checkout:

```bash
git clone --recursive https://github.com/tadgem/luajit-cmake.git
# or, in an existing checkout:
git submodule update --init --recursive
```

`LUAJIT_DIR` then defaults to the bundled `LuaJIT/` directory and does not
need to be set. Pass `-DLUAJIT_DIR=...` to build against another LuaJIT tree.

## External Dependencies

This project requires several external tools for building and cross-compilation:

### MinGW-w64

- **Purpose**: Used as a cross-compilation toolchain for building LuaJIT for Windows targets.
- **Toolchain File**: `Utils/windows.toolchain.cmake`
- **Installation**: Install MinGW-w64 from [mingw-w64.org](https://www.mingw-w64.org/)

### Git

- **Purpose**: Required for cloning the repository and managing source code.
- **Installation**: Usually pre-installed on macOS. For other systems, install from [git-scm.com](https://git-scm.com/)

### Wine

- **Purpose**: Used to run 32-bit Windows executables on non-Windows systems during the build process.
- **Usage**: Referenced in `Utils/Darwin.wine.cmake` for macOS builds.
- **Installation**: Install Wine from [winehq.org](https://www.winehq.org/)

## Build

### make

Use a GNU compatible make.

`make` or `mingw32-make` or `gnumake`.

_Note_: `LUAJIT_DIR` can still be overridden, e.g.
`make LUAJIT_DIR=/path/to/LuaJIT`. When using mingw32-make, please change
`\\` to `/` in file paths on Windows.

### cmake

Use cmake to compile.

```bash
cmake -H. -Bbuild
make --build build --config Release
```

### Embed

```cmake
add_subdirectory(luajit-cmake)
target_link_libraries(yourTarget PRIVATE luajit::lib luajit::header)
```

Look samples at [lua-forge](https://github.com/zhaozg/lua-forge/blob/master/CMakeLists.txt)

### Demo

The `demo/` directory contains a small program that embeds LuaJIT, exposes
C functions to Lua, and runs a Lua script which calls them.

Build it standalone:

```bash
cmake -S demo -B build-demo
cmake --build build-demo --config Release
# Visual Studio: build-demo/Release/luajit_demo.exe
# single-config generators: build-demo/luajit_demo[.exe]
```

Or build it as part of this project:

```bash
cmake -H. -Bbuild -DLUAJIT_BUILD_DEMO=ON
cmake --build build --config Release
```

### CrossCompile

#### iOS

```bash
make iOS
```

#### Android

```bash
make Android
```

#### Windows

```bash
make Windows
```

#### HarmonyOS

The library also supports cross-compilation for HarmonyOS. Please refer to the toolchain files in `Utils/` directory for HarmonyOS specific configurations.

```bash
make OHOS
```

#### Note

_Note_: The i386 architecture is deprecated for macOS (remove from the Xcode
build setting: ARCHS). So I use mingw-w64 and wine to build and run 32 bits
minilua and buildvm.
