cmake_minimum_required(VERSION 3.20)

# WIN32XX_VERSION carries the full build version. CI injects the
# four-component value 9.6.1.{buildnumber} via -DWIN32XX_VERSION=...; a local
# build that does not pass the option falls back to the base release version.
if(NOT DEFINED WIN32XX_VERSION OR WIN32XX_VERSION STREQUAL "")
    set(WIN32XX_VERSION "9.6.1")
endif()

project(win32xx VERSION "${WIN32XX_VERSION}" LANGUAGES CXX)

# Win32++ is a header-only framework, so the CMake target only needs to expose
# the include directory. Consumers keep using the existing headers:
#   #include "wxx_wincore.h"
add_library(win32xx INTERFACE)
add_library(win32xx::win32xx ALIAS win32xx)

target_include_directories(win32xx
        INTERFACE
        "${CMAKE_CURRENT_SOURCE_DIR}/include")

# Win32++ is a TCHAR-based framework. This package compiles consumers in
# Unicode by default: linking win32xx::win32xx defines UNICODE and _UNICODE so
# the Windows API and Win32++ resolve to their wide-character variants.
target_compile_definitions(win32xx
        INTERFACE
        UNICODE
        _UNICODE)
