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

# This portion defines the steps required to build and link libxmdf.a as well
# as the target installation directory for the resulting library and header
# files.

# Define source files that are to be compiled into libxmdf.
SET( XMDF_SRCS
  Xmdf.c
  Xmdf_f.c
  xmdf_private.c
  xmdf_timestep.c
  Xmdff.f90
)


# Set up definitions that affect source code compilation and add include
# directories so header files can be located.
INCLUDE_DIRECTORIES( ${HDF5_INCLUDE_DIR} ${XMDF_SOURCE_DIR}/include ${CMAKE_CURRENT_SOURCE_DIR}/include )

# This is disabled because the XMDF_API is a preprocessor macro that defines
# DLL visibility for library functions. Setting DLL attributes on UNIX causes
# errors.
ADD_DEFINITIONS( -DNO_XMDF_API )

# Ifort requires lower case symbol names in order to cross-link.
IF("${CMAKE_Fortran_COMPILER_ID}" MATCHES "Intel")
  ADD_DEFINITIONS( -DFLOWER )
ENDIF()


ADD_LIBRARY( xmdf ${XMDF_SRCS} )
TARGET_LINK_LIBRARIES( xmdf ${HDF5_LIBRARIES} )

# Specify install location for the libraries which will be
# prefix/library/install/location.  In this case the location for libraries is
# specified to be:
#
#     prefix/lib
#
# Also, export a description of this libray for use by other CMake scripts.
INSTALL(
  TARGETS xmdf EXPORT XMDFLibraryDepends
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
)


# Fortran module files are tricky---they are generated by the Fortran compiler
# when it processes the .f90 files but need to be installed into an include
# directory for future use. Therefore, we set the Fortran_MODULE_DIRECTORY
# property of the xmdf library such that it dumps all these files into a
# subdirectory. This makes the directory contents easy to install using an
# INSTALL DIRECTORY command.
SET_TARGET_PROPERTIES( xmdf
  PROPERTIES
    Fortran_MODULE_DIRECTORY fortran_modules
)
INSTALL(
  DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/fortran_modules/
  DESTINATION include/xmdf
)

# Install C include files to prefix/include
INSTALL(
  DIRECTORY
    ${XMDF_SOURCE_DIR}/include/xmdf
  DESTINATION include
)
