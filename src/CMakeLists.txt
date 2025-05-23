#
#   This file is part of Magnum.
#
#   Original authors — credit is appreciated but not required:
#
#       2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018, 2019,
#       2020, 2021, 2022, 2023, 2024, 2025
#            — Vladimír Vondruš <mosra@centrum.cz>
#       2019 — Jonathan Hale <squareys@googlemail.com>
#
#   This is free and unencumbered software released into the public domain.
#
#   Anyone is free to copy, modify, publish, use, compile, sell, or distribute
#   this software, either in source code form or as a compiled binary, for any
#   purpose, commercial or non-commercial, and by any means.
#
#   In jurisdictions that recognize copyright laws, the author or authors of
#   this software dedicate any and all copyright interest in the software to
#   the public domain. We make this dedication for the benefit of the public
#   at large and to the detriment of our heirs and successors. We intend this
#   dedication to be an overt act of relinquishment in perpetuity of all
#   present and future rights to this software under copyright law.
#
#   THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
#   IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
#   FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL
#   THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
#   IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
#   CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
#

# On MSVC remove /W3, as we are replacing it with /W4. Could be removed as of
# 3.15 with this: https://cmake.org/cmake/help/latest/policy/CMP0092.html
if(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC" OR CMAKE_CXX_SIMULATE_ID STREQUAL "MSVC")
    string(REPLACE "/W3" "" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
endif()

# On Windows enable UNICODE/_UNICODE macros to avoid accidentally passing UTF-8
# values to ANSI functions
if(CORRADE_TARGET_WINDOWS)
    add_definitions("-DUNICODE" "-D_UNICODE")
endif()

# IDE folder in VS, Xcode etc. CMake 3.12+, older versions have only the FOLDER
# property that would have to be set on each target separately.
set(CMAKE_FOLDER "MagnumExamples")

if(MAGNUM_WITH_ANIMATED_GIF_EXAMPLE)
    add_subdirectory(animated-gif)
endif()

if(MAGNUM_WITH_ARCBALL_EXAMPLE)
    add_subdirectory(arcball)
endif()

if(MAGNUM_WITH_AREALIGHTS_EXAMPLE)
    add_subdirectory(arealights)
endif()

if(MAGNUM_WITH_AUDIO_EXAMPLE)
    add_subdirectory(audio)
endif()

if(MAGNUM_WITH_BOX2D_EXAMPLE)
    add_subdirectory(box2d)
endif()

if(MAGNUM_WITH_BULLET_EXAMPLE)
    add_subdirectory(bullet)
endif()

if(MAGNUM_WITH_CUBEMAP_EXAMPLE)
    add_subdirectory(cubemap)
endif()

if(MAGNUM_WITH_DART_EXAMPLE)
    add_subdirectory(dart)
endif()

if(MAGNUM_WITH_FLUIDSIMULATION2D_EXAMPLE)
    add_subdirectory(fluidsimulation2d)
endif()

if(MAGNUM_WITH_FLUIDSIMULATION3D_EXAMPLE)
    add_subdirectory(fluidsimulation3d)
endif()

if(MAGNUM_WITH_IMGUI_EXAMPLE)
    add_subdirectory(imgui)
endif()

if(MAGNUM_WITH_LEAPMOTION_EXAMPLE)
    add_subdirectory(leapmotion)
endif()

if(MAGNUM_WITH_MOTIONBLUR_EXAMPLE)
    add_subdirectory(motionblur)
endif()

if(MAGNUM_WITH_MOUSEINTERACTION_EXAMPLE)
    add_subdirectory(mouseinteraction)
endif()

if(MAGNUM_WITH_OCTREE_EXAMPLE)
    add_subdirectory(octree)
endif()

if(MAGNUM_WITH_OVR_EXAMPLE)
    add_subdirectory(ovr)
endif()

if(MAGNUM_WITH_PICKING_EXAMPLE)
    add_subdirectory(picking)
endif()

if(MAGNUM_WITH_PRIMITIVES_EXAMPLE)
    add_subdirectory(primitives)
endif()

if(MAGNUM_WITH_RAYTRACING_EXAMPLE)
    add_subdirectory(raytracing)
endif()

if(MAGNUM_WITH_SHADOWS_EXAMPLE)
    add_subdirectory(shadows)
endif()

if(MAGNUM_WITH_TEXT_EXAMPLE)
    add_subdirectory(text)
endif()

if(MAGNUM_WITH_TEXTUREDQUAD_EXAMPLE)
    add_subdirectory(texturedquad)
endif()

if(MAGNUM_WITH_TEXTUREDTRIANGLE_VULKAN_EXAMPLE)
    add_subdirectory(textured-triangle-vulkan)
endif()

if(MAGNUM_WITH_TRIANGLE_EXAMPLE)
    add_subdirectory(triangle)
endif()

if(MAGNUM_WITH_TRIANGLE_PLAIN_GLFW_EXAMPLE)
    add_subdirectory(triangle-plain-glfw)
endif()

if(MAGNUM_WITH_TRIANGLE_SOKOL_EXAMPLE)
    add_subdirectory(triangle-sokol)
endif()

if(MAGNUM_WITH_TRIANGLE_VULKAN_EXAMPLE)
    add_subdirectory(triangle-vulkan)
endif()

if(MAGNUM_WITH_VIEWER_EXAMPLE)
    add_subdirectory(viewer)
endif()

if(MAGNUM_WITH_WEBXR_EXAMPLE)
    add_subdirectory(webxr)
endif()
