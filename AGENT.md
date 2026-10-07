# luajit-cmake

## Overview

This repository provides a flexible CMake-based build system for LuaJIT, supporting various platforms and cross-compilation scenarios.

LuaJIT is bundled as a git submodule in `LuaJIT/`. `LUAJIT_DIR` defaults to that directory, so it does not need to be provided by the user. Initialize the submodule with `git submodule update --init --recursive`.

## External Dependencies

The project requires several external tools for building and cross-compilation:

### MinGW-w64

- **Purpose**: Used as a cross-compilation toolchain for building LuaJIT for Windows targets.
- **Toolchain File**: `Utils/windows.toolchain.cmake`
- **Installation**: Install MinGW-w64 from [mingw-w64.org](https://www.mingw-w64.org/)

### Wine

- **Purpose**: Used to run 32-bit Windows executables on non-Windows systems during the build process.
- **Usage**: Referenced in `Utils/Darwin.wine.cmake` for macOS builds.
- **Installation**: Install Wine from [winehq.org](https://www.winehq.org/)

## Supported Platforms

- Native builds (Linux, macOS, Windows)
- iOS cross-compilation
- Android cross-compilation
- Windows cross-compilation from other platforms
- HarmonyOS support

## Build Instructions

Refer to `readme.md` for detailed build instructions using make or CMake.

## Repository Structure

- Root directory contains main CMake files and build scripts
- `LuaJIT/`: git submodule containing the LuaJIT source (the default `LUAJIT_DIR`)
- `Utils/`: Platform-specific toolchain files including MinGW-w64 and Wine configurations
- `Modules/`: CMake modules for finding dependencies
- `host/`: Contains subdirectories for host tools (buildvm, minilua)
- `demo/`: Small application embedding LuaJIT and exposing C functions to Lua

This analysis identifies all external dependencies and their roles in the build system, providing clear documentation for users who wish to set up their environment for building LuaJIT with this CMake configuration.
