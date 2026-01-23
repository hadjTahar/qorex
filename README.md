# Qorex

Qorex as in Core Graphics, Qx as a shortcut, is a C++ game engine, app development framewrok in the making, based on:

- SDL3 windowing system
- 2D multiple rendering backends
	- SKIA ![WIP](https://img.shields.io/badge/WIP-red?style=flat-square)
	- SDL renderer ![WIP](https://img.shields.io/badge/WIP-red?style=flat-square)
	- ThorVG ❌
	- Blend2D ❌
- 3D multiple rendering backends
	- Google filament ❌
	- bgfx ❌
	- magnum ❌

**For more information check docs folder**

### How to build

#### Skia backend

- The hardest part is compiling skia, once you build it (skia, good luck with that)
- Run "cmake" on the project, it will fail, but will generate a folder at "vendors/skia/libs/FOLDER" where it expects skia library to be found, something like
	- skia cpu : "vendors/skia/libs/windows10_clang_release_x64_skia_raster"
 	- skia opengl : "vendors/skia/libs/windows10_clang_release_x64_skia_opengl"
    - skia direct3D : "vendors/skia/libs/windows10_clang_release_x64_skia_direct3d"
    - skia vulkan : "vendors/skia/libs/windows10_clang_release_x64_skia_vulkan"
- Copy the generated skia.(lib, a, ..) into that folder
- Run cmake again

Tha folder changes depending on the "os_comiler_compiler-mode_arch_skia-backend", check skia.md, on how I built it.


### License:

Currently GPL, may definitely change in the future
