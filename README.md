XMDF-unix
=========

This is a re-distribution of the XMDF library produced by the Environmental
Modeling Research Laboratory and Brigham Young University which has been
modified to allow for compilation and use in UNIX environments.

The original source may be obtained from:

  http://www.xmdf.org

The library may be built using the CMake build system:

    cd build
    cmake .. -DCMAKE_INSTALL_PREFIX=/path/to/installation/directory
    make install

The CMake build system is available at:

  http://www.cmake.org


Changes
-------

### Releases based on XMDF 1.9

Compared to the original source code for XMDF version 1.9, the following
changes have been made:


#### XMDF-unix 1.9-1

  * The project has been re-organized to build using CMake instead of
    VisualStudio.

  * The file `xmdf_private.c` has been edited to remove references to
    `H5Eget_free`, a function which is no longer part of the HDF5 API.
    This function has been replaced by a plain call to `free`.

