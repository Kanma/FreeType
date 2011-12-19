cmake_minimum_required(VERSION 2.6)
project(FREETYPE)


##########################################################################################
# XMake-related settings

# Set the output paths
if (NOT DEFINED XMAKE_BINARY_DIR)
    set(XMAKE_BINARY_DIR "${FREETYPE_BINARY_DIR}")
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${XMAKE_BINARY_DIR}/bin")
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${XMAKE_BINARY_DIR}/lib")
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${XMAKE_BINARY_DIR}/bin")
endif()

# Set the dependencies path
if (NOT DEFINED XMAKE_DEPENDENCIES_DIR)
    set(XMAKE_DEPENDENCIES_DIR "${FREETYPE_SOURCE_DIR}/dependencies")
endif()


##########################################################################################
# XMake importation

if (NOT EXISTS "${XMAKE_DEPENDENCIES_DIR}/XMake/XMake.cmake")
    message(FATAL_ERROR
"Missing dependency: XMake
This distribution of FreeType requires XMake as a dependency.

It is provided as GIT submodule of this repository.

Did you forgot to execute the following commands?
   git submodule init
   git submodule update")
endif()

include("${XMAKE_DEPENDENCIES_DIR}/XMake/XMake.cmake")


##########################################################################################
# Declaration of the library

# List the source files
set(SRCS src/autofit/autofit.c
         src/base/ftbase.c
         src/base/ftbbox.c
         src/base/ftbitmap.c
         src/base/ftfstype.c
         src/base/ftgasp.c
         src/base/ftglyph.c
         src/base/ftinit.c
         src/base/ftmm.c
         src/base/ftpfr.c
         src/base/ftstroke.c
         src/base/ftsynth.c
         src/base/ftsystem.c
         src/base/fttype1.c
         src/base/ftwinfnt.c
         src/bdf/bdf.c
         src/cache/ftcache.c
         src/cff/cff.c
         src/cid/type1cid.c
         src/gzip/ftgzip.c
         src/lzw/ftlzw.c
         src/pcf/pcf.c
         src/pfr/pfr.c
         src/psaux/psaux.c
         src/pshinter/pshinter.c
         src/psnames/psmodule.c
         src/raster/raster.c
         src/sfnt/sfnt.c
         src/truetype/truetype.c
         src/type1/type1.c
         src/type42/type42.c
         src/winfonts/winfnt.c
)

# List the include paths
set(INCLUDE_PATHS "${FREETYPE_SOURCE_DIR}/include")

include_directories(${INCLUDE_PATHS})

# Declaration of the library
xmake_create_static_library(FREETYPE freetype ${SRCS})

xmake_add_to_list_property(FREETYPE COMPILE_DEFINITIONS "FT2_BUILD_LIBRARY")

if (NOT WIN32)
    xmake_add_to_property(FREETYPE COMPILE_FLAGS "-w")
else()
    xmake_add_to_property(FREETYPE COMPILE_FLAGS "/W0")
    xmake_add_to_list_property(FREETYPE COMPILE_DEFINITIONS "_CRT_SECURE_NO_DEPRECATE")
endif()

# Exportations
xmake_export_include_paths(FREETYPE ${INCLUDE_PATHS})
