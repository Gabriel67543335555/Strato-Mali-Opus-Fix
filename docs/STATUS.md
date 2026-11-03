# AetherCore engineering notes

## Initial repository audit

This repository began with a single README and no C++ source tree. It therefore qualifies as a repository that needed initialization under the AetherCore recovery prompt. The project was initialized conservatively: the existing README was kept and expanded, and a minimal host-core foundation was added.

## Included in the current milestone

- CMake project configured for host builds with CTest
- aethercore library with guest-memory read/write semantics
- CPU-backend abstraction for future Dynarmic/NCE separation
- minimal test coverage for memory bounds and round-trips
- build script for local host builds

## Real status

No local shell or build toolchain is available to this GitHub API session, so this record intentionally does not claim that the project built or passed tests here. The implementation is prepared for a standard CMake/CTest flow, but it remains an unvalidated host foundation until the build is executed in a proper environment.

## Next milestone

Pin a real Dynarmic upstream revision, implement the actual AArch64 JIT adapter, and add an instruction-execution test that verifies register and memory state before claiming CPU execution is functional.
