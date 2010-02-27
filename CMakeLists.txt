cmake_minimum_required(VERSION 2.6)
project(FREETYPE)

# Options
if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose the type of build, options are: None (CMAKE_CXX_FLAGS or CMAKE_C_FLAGS used) Debug Release RelWithDebInfo MinSizeRel." FORCE)
endif()

# Set the output path
if (NOT ATHENA_BINARY_DIR)
    set(ATHENA_BINARY_DIR "${FREETYPE_BINARY_DIR}")
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${FREETYPE_BINARY_DIR}/lib")
endif()

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
include_directories(include)

# Declaration of the library
add_library(freetype STATIC ${SRCS})

# Compilation settings
set_target_properties(freetype PROPERTIES COMPILE_DEFINITIONS "_CRT_SECURE_NO_WARNINGS;FT2_BUILD_LIBRARY")

if (NOT WIN32)
    set_target_properties(freetype PROPERTIES COMPILE_FLAGS "-w")
else()
    set_target_properties(freetype PROPERTIES COMPILE_FLAGS "/W0")
endif()
