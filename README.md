XMDF-unix
=========

This is a re-distribution of the XMDF library. No modifications have been made
to the distributed source code, but the directory structure has been
re-organized and a new build system has been added which enables the library to
be compiled and used in UNIX-Like environments.

The original source may be obtained from:

  http://www.xmdf.org

To build the library:

    cd build
    cmake .. -DCMAKE_INSTALL_PREFIX=/path/to/installation/directory
    make install

The CMake build system is available at:

  http://www.cmake.org

