XMDF-unix
=========

This is an unofficial re-distribution of the [XMDF library][xmdf] that has been
ported to run on UNIX-like systems such as Linux and Mac OS X. XMDF is a
library produced by the Environmental Modeling Research Laboratory and Brigham
Young University which is designed to store scientific data. According to the
XMDF website:

> XMDF is a C and Fortran language library providing a standard format for the
> geometry data storage of river cross-sections, 2D/3D structured grids, 2D/3D
> unstructured meshes, geometric paths through space, and associated time data.

The original source may be obtained from:

  http://www.xmdf.org

XMDF is implemented on top of the [HDF5 library][hdf], so HDF5 must be
installed first. After installing HDF5, the XMDF library may be compiled using
the CMake build system:

    cd build
    cmake .. -DCMAKE_INSTALL_PREFIX=/path/to/installation/directory
    make install

The CMake build system is available at:

  http://www.cmake.org

  [xmdf]: http://www.xmdf.org
  [hdf]: http://www.hdfgroup.org/HDF5


Changes
-------

### Releases based on XMDF 1.9

Compared to the original source code for XMDF version 1.9, the following
changes have been made:

#### XMDF-unix 1.9-2

  * The Fortran module is now built and installed.

  * Config files are installed that help other CMake-based projects use XMDF.

#### XMDF-unix 1.9-1

  * The project has been re-organized to use CMake as a buildsystem instead of
    VisualStudio.

  * The file `xmdf_private.c` has been edited to remove references to
    `H5Eget_free`, a function which is no longer part of the HDF5 API.
    This function has been replaced by a plain call to `free`.

