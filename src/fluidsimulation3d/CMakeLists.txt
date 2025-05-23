#
#   This file is part of Magnum.
#
#   Original authors — credit is appreciated but not required:
#
#       2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018, 2019,
#       2020, 2021, 2022, 2023, 2024, 2025
#            — Vladimír Vondruš <mosra@centrum.cz>
#       2019 — Nghia Truong <nghiatruong.vn@gmail.com>
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

cmake_minimum_required(VERSION 3.5...3.10)

project(FluidSimulation3DExample CXX)

# Add module path in case this is project root
if(PROJECT_SOURCE_DIR STREQUAL CMAKE_SOURCE_DIR)
    set(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/../../modules/" ${CMAKE_MODULE_PATH})
endif()

find_package(Corrade REQUIRED Main)
find_package(Magnum REQUIRED
    GL
    MeshTools
    Primitives
    SceneGraph
    Shaders
    Sdl2Application)
find_package(MagnumIntegration REQUIRED ImGui)

set_directory_properties(PROPERTIES CORRADE_USE_PEDANTIC_FLAGS ON)

option(MAGNUM_FLUIDSIMULATION3D_EXAMPLE_USE_MULTITHREADING "Build FluidSimulation example with parallel computation" ON)
option(MAGNUM_FLUIDSIMULATION3D_EXAMPLE_USE_TBB "Using Intel TBB if FluidSimulation is built with parallel computation enabled" OFF)

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/configure.h.cmake
               ${CMAKE_CURRENT_BINARY_DIR}/configure.h)

corrade_add_resource(FluidSimulation_RESOURCES resources.conf)

add_executable(magnum-fluidsimulation3d WIN32
    FluidSimulation3DExample.cpp
    TaskScheduler.h
    ThreadPool.h
    DrawableObjects/WireframeObjects.h
    DrawableObjects/FlatShadeObject.h
    DrawableObjects/ParticleGroup.h
    DrawableObjects/ParticleGroup.cpp
    SPH/DomainBox.h
    SPH/DomainBox.cpp
    SPH/SPHKernels.h
    SPH/SPHSolver.h
    SPH/SPHSolver.cpp
    Shaders/ParticleSphereShader.h
    Shaders/ParticleSphereShader.cpp
    ${FluidSimulation_RESOURCES})
target_link_libraries(magnum-fluidsimulation3d PRIVATE
    Corrade::Main
    Magnum::Application
    Magnum::GL
    Magnum::Magnum
    Magnum::MeshTools
    Magnum::Primitives
    Magnum::SceneGraph
    Magnum::Shaders
    MagnumIntegration::ImGui)
target_include_directories(magnum-fluidsimulation3d PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR})
if(MAGNUM_FLUIDSIMULATION3D_EXAMPLE_USE_MULTITHREADING)
    find_package(Threads REQUIRED)
    target_link_libraries(magnum-fluidsimulation3d PRIVATE Threads::Threads)
endif()
if(MAGNUM_FLUIDSIMULATION3D_EXAMPLE_USE_TBB)
    # TBBConfig.cmake adds -isystem /usr/lib/cmake/TBB/../../../include, which
    # breaks compilation. Temporary workaround by not including that dir as
    # system, see https://github.com/intel/tbb/issues/195 and
    # https://github.com/intel/tbb/pull/196
    set_target_properties(magnum-fluidsimulation3d PROPERTIES
        NO_SYSTEM_FROM_IMPORTED ON)
    find_package(TBB CONFIG REQUIRED)
    target_link_libraries(magnum-fluidsimulation3d PRIVATE TBB::tbb)
endif()

install(TARGETS magnum-fluidsimulation3d DESTINATION ${MAGNUM_BINARY_INSTALL_DIR})

# Make the executable a default target to build & run in Visual Studio
set_property(DIRECTORY ${PROJECT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT magnum-fluidsimulation3d)
