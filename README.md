# AetherCore

AetherCore is an early Nintendo Switch emulator core project. The repository previously contained only a one-line project README; this initial implementation adds a small, buildable host-side foundation without replacing that history.

## Current milestone

- C++20 static library and CMake/CTest host build.
- Bounded guest-memory read/write implementation with overflow-safe range checks.
- Abstract CPU-backend contract to keep execution engines independent.
- Optional CMake FetchContent declaration for the official Dynarmic repository (`https://github.com/merryhime/dynarmic`). Fetching it is opt-in via `-DAETHERCORE_FETCH_DYNARMIC=ON`.

**Important limitation:** the Dynarmic source-fetch configuration is not an execution integration. No Dynarmic adapter, JIT execution, ARM64 instruction test, memory protection/page tables, NSO loader, NCE, Android JNI, or Vulkan path is implemented yet. Do not treat those features as working. The upstream dependency is currently set to the moving `master` branch because a specific upstream commit could not be verified in this environment; pin a reviewed commit before relying on reproducible Dynarmic builds.

## Host build and tests

```sh
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release -DAETHERCORE_BUILD_TESTS=ON
cmake --build build --parallel
ctest --test-dir build --output-on-failure
```

The memory test prints individual PASS/FAIL results. No test is marked SKIPPED or passed unless CTest actually runs it. Dynarmic tests are not present yet.

## Android target direction

CMake supports the `AETHERCORE_EXYNOS_1330` profile flag, but Android packaging is not configured. For a future NDK build, use `ANDROID_ABI=arm64-v8a` and a supported Android NDK toolchain; this profile currently only records a compile definition and does not claim device-specific optimization or Galaxy A14 compatibility.

## Next technical milestone

Verify and pin an upstream Dynarmic revision, implement its actual A64 callback/JIT adapter against that revision's headers, and add an instruction-execution test that checks resulting register/memory state before describing the backend as functional.
