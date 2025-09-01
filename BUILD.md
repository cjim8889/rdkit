Build Guide (vcpkg)

Overview
- This project supports building with vcpkg-managed dependencies via the included `vcpkg.json` manifest.
- CMake minimum in the project is 3.18. CMake presets in this repo require CMake 3.21+. If your CMake is older, use the manual configure command shown below.

Prerequisites
- Git
- CMake 3.18+
- A supported compiler toolchain (e.g., MSVC, clang, or GCC)

Install vcpkg
- Clone next to the source tree and bootstrap:
  - Unix/macOS:
    - `git clone https://github.com/microsoft/vcpkg.git`
    - `./vcpkg/bootstrap-vcpkg.sh`
  - Windows (PowerShell):
    - `git clone https://github.com/microsoft/vcpkg.git`
    - `.\vcpkg\bootstrap-vcpkg.bat`

Configure
- Using CMake Presets (requires CMake 3.21+):
  - `cmake --preset vcpkg-release`
  - or `cmake --preset vcpkg-debug`

- Manual configure (works with older CMake):
  - `cmake -S . -B build/vcpkg-release -DCMAKE_TOOLCHAIN_FILE=./vcpkg/scripts/buildsystems/vcpkg.cmake -DVCPKG_FEATURE_FLAGS=manifests`

Build
- `cmake --build build/vcpkg-release -j`

Manifest dependencies
- The manifest (`vcpkg.json`) includes:
  - `boost-headers`, `boost-iostreams`, `boost-serialization`
  - `eigen3`, `catch2`, `better-enums`
- These cover the default configuration without Python wrappers. Optional graphics libs (e.g., `cairo`, `freetype`, `libpng`) are not included by default.

Common options
- Disable Python wrappers (avoids Boost.Python and Python toolchain via vcpkg):
  - `-DRDK_BUILD_PYTHON_WRAPPERS=OFF`
- Enable Cairo drawing support (managed by vcpkg):
  - Add to `vcpkg.json`: `cairo`, `freetype`, `libpng`
  - Re-configure and rebuild.
- Example depending on Boost.ProgramOptions (optional C++ test/example):
  - Add `boost-program-options` to `vcpkg.json` to provide `Boost::program_options`.

Better Enums
- The build prefers a preinstalled/header-only `better-enums` (via vcpkg) and only fetches it as a fallback.

Notes
- Some optional third-party sources (e.g., maeparser, coordgenlibs) are downloaded by the project’s CMake when corresponding features are enabled. Disable those features if you need a fully offline build.
- To change build type with manual configure, pass `-DCMAKE_BUILD_TYPE=Debug` (or `Release`).

Troubleshooting
- Presets not recognized:
  - Use the manual configure command above if your CMake is older than 3.21.
- Missing Boost component warnings (e.g., `boost_program_options`):
  - Add the component to `vcpkg.json` and re-configure.
- Python wrappers build pulls in `python3` port and fails on your environment:
  - Configure with `-DRDK_BUILD_PYTHON_WRAPPERS=OFF`.

