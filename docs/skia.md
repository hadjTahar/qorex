# To build skia

## Init
---

- Download skia into a short path like "D:/" or "C:/"
	- git clone https://github.com/google/skia.git
- use "x86/x64 native tools for VS"
- cd skia
- python tools/git-sync-deps --verbose
- Run it two times


## Gn
---

- On windows, open native tools, either x86 or x64, not cmd or powershell
- .\bin\gn.exe gen out/build
- Choose from args.gn
- Copy the text to out/build/args.gn
- Run ".\bin\gn.exe gen out/build" again
- ninja -C out/build skia
- Check build arch
	- windows: dumpbin /headers out\build\skia.lib | findstr "machine"
- Copy the .lib to "vendors/skia/lib"



#### Clang args.gn on windows - windows10_clang_release_x64___:

	# Set build arguments here. See `gn help buildargs`.
	target_cpu = "x64"
	cc = "clang-cl"
	cxx = "clang-cl"
	# Use forward slashes here!
	clang_win = "C:/Program Files/Microsoft Visual Studio/18/Community/VC/Tools/Llvm/x64"
	is_official_build = true
	is_component_build = false 
	skia_use_system_libjpeg_turbo = false
	skia_use_system_zlib = false
	skia_use_system_harfbuzz = false
	skia_use_system_libpng = false
	skia_use_system_libwebp = false 
	skia_use_system_expat = false
	skia_use_system_icu = false
	skia_use_icu = false
	skia_enable_tools = false
	Select backend

#### MSVC args.gn on windows - windows10_msvc_release_x64___:

	# Set build arguments here. See `gn help buildargs`.
	target_cpu = "x64"
	is_official_build = true
	is_component_build = false 
	skia_use_system_libjpeg_turbo = false
	skia_use_system_zlib = false
	skia_use_system_harfbuzz = false
	skia_use_system_libpng = false
	skia_use_system_libwebp = false 
	skia_use_system_expat = false
	skia_use_system_icu = false
	skia_use_icu = false
	skia_enable_tools = false
	Select backend

#### Build mismatch

Sometimes gn defaults to x86, if there is a build mismatch, open toolchain.ninja, and check if command(s) are pointing to the right arch x86/x64 folder.
- x86 : "C:/Program Files/Microsoft Visual Studio/18/Community/VC/Tools/Llvm/bin"
- x64 : "C:/Program Files/Microsoft Visual Studio/18/Community/VC/Tools/Llvm/x64/bin"



#### GPU backend flags

| Flag                     | Purpose                          |
| ------------------------ | -------------------------------- |
| `skia_use_gl=true`       | Enable **OpenGL GPU backend**    |
| `skia_use_vulkan=true`   | Enable **Vulkan GPU backend**    |
| `skia_use_metal=true`    | Enable **Apple Metal backend**   |
| `skia_use_direct3d=true` | Enable **Direct3D backend**      |
| `skia_use_dawn=true`     | Enable **WebGPU (Dawn)** backend |


## Bazel


Bazel is the recommended way to build skia, but I was not sucessfull
