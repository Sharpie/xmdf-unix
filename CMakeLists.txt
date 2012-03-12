# CMake control file for building the XMDF library
# Copyright (C) 2012 Charlie Sharpsteen
#
# Permission is hereby granted, free of charge, to any person obtaining a copy of
# this software and associated documentation files (the "Software"), to deal in
# the Software without restriction, including without limitation the rights to
# use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
# of the Software, and to permit persons to whom the Software is furnished to do
# so, subject to the following conditions:
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.


# This is a top-level file that sets up dependencies. Building and
# installation of the source code is handled by the CMakeLists.txt file in the
# src subdirectory.

# Will probably work with older versions of CMake, but has not been tested.
# Therefore the minimum version is set to 2.8 but this is a "soft constraint"
# Since this project uses Fortran source code, CMake >= 2.6 is probably a hard
# constraint as that is when Fortran support was finalized in CMake.
CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
PROJECT( XMDF )

SET(CMAKE_COLOR_MAKEFILE ON)
SET(CMAKE_INCLUDE_CURRENT_DIR ON)

# Default to a release build
IF ( NOT CMAKE_BUILD_TYPE )
  SET(CMAKE_BUILD_TYPE "Release")
ENDIF ()

SET(XMDF_MAJOR_VERSION 1)
SET(XMDF_MINOR_VERSION 9)
SET(XMDF_PATCH_VERSION 0)
SET(XMDF_UNIX_RELEASE 2)
SET(XMDF_VERSION_STRING
  "${XMDF_MAJOR_VERSION}.${XMDF_MINOR_VERSION}.${XMDF_PATCH_VERSION}-${XMDF_UNIX_RELEASE}")

# Gather library requirements-- these are a Fortran compiler and a recent
# version of the HDF5 library.
ENABLE_LANGUAGE( Fortran )
FIND_PACKAGE( HDF5 1.8.1 COMPONENTS C REQUIRED )


# Descend into the src directory to build and install the library.
SUBDIRS( src )


# Create configuration files to help other programs find the library
# after installation.
CONFIGURE_FILE(
  "${XMDF_SOURCE_DIR}/XMDFConfig.cmake.in"
  "${XMDF_BINARY_DIR}/XMDFConfig.cmake"
  @ONLY
)
CONFIGURE_FILE(
  "${XMDF_SOURCE_DIR}/XMDFConfigVersion.cmake.in"
  "${XMDF_BINARY_DIR}/XMDFConfigVersion.cmake"
  @ONLY
)

INSTALL(
  EXPORT XMDFLibraryDepends
  DESTINATION "lib/cmake/XMDF-${XMDF_VERSION_STRING}"
  COMPONENT dev
)

INSTALL(
  FILES "${XMDF_BINARY_DIR}/XMDFConfig.cmake"
  DESTINATION "lib/cmake/XMDF-${XMDF_VERSION_STRING}"
  COMPONENT dev
)

INSTALL(
  FILES "${XMDF_BINARY_DIR}/XMDFConfigVersion.cmake"
  DESTINATION "lib/cmake/XMDF-${XMDF_VERSION_STRING}"
  COMPONENT dev
)
