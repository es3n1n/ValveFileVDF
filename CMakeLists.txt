cmake_minimum_required (VERSION 3.6)
project (vdf-Parser)

add_library(ValveFileVDF INTERFACE)
add_library(ValveFileVDF::ValveFileVDF ALIAS ValveFileVDF)
target_include_directories(ValveFileVDF INTERFACE "include")

# Disable cxx17 deprecation warnings for the std::wstring_convert
target_compile_definitions(ValveFileVDF INTERFACE "-D_LIBCPP_DISABLE_DEPRECATION_WARNINGS")

include(CTest)
add_subdirectory(./tests)

## add fuzzing, if supported

set(ENABLE_FUZZING OFF)
if ("${CMAKE_CXX_COMPILER_ID}" MATCHES "Clang" OR "${CMAKE_CXX_COMPILER_ID}" MATCHES "MSVC" AND NOT APPLE)
	set(ENABLE_FUZZING ON)
endif()

if (${ENABLE_FUZZING})
    add_subdirectory(fuzzing)
endif()
