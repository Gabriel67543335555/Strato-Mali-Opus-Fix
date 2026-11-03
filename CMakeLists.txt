cmake_minimum_required(VERSION 3.20)
project(AetherCore VERSION 0.1.0 LANGUAGES CXX)

option(AETHERCORE_BUILD_TESTS "Build AetherCore host tests" ON)
option(AETHERCORE_EXYNOS_1330 "Enable the ARM64 Exynos 1330 profile definition" OFF)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

add_library(aethercore src/guest_memory.cpp)
target_include_directories(aethercore PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include")

if(AETHERCORE_EXYNOS_1330)
  target_compile_definitions(aethercore PUBLIC AETHERCORE_EXYNOS_1330=1)
endif()

if(MSVC)
  target_compile_options(aethercore PRIVATE /W4 /WX)
else()
  target_compile_options(aethercore PRIVATE -Wall -Wextra -Wpedantic -Werror)
endif()

if(AETHERCORE_BUILD_TESTS)
  enable_testing()
  add_executable(aethercore_memory_tests tests/guest_memory_test.cpp)
  target_link_libraries(aethercore_memory_tests PRIVATE aethercore)
  if(MSVC)
    target_compile_options(aethercore_memory_tests PRIVATE /W4 /WX)
  else()
    target_compile_options(aethercore_memory_tests PRIVATE -Wall -Wextra -Wpedantic -Werror)
  endif()
  add_test(NAME aethercore.guest_memory COMMAND aethercore_memory_tests)
endif()
