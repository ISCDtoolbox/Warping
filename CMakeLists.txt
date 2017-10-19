cmake_minimum_required(VERSION 2.8)
set(CMAKE_BUILD_TYPE  "Release")
set(CMAKE_C_FLAGS   "-w -O3")
set(CMAKE_MACOSX_RPATH 1)
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

project(libWarping)
file(                  GLOB_RECURSE sources src/*)
file(                  GLOB_RECURSE headers src/*.h)
add_library(           libWarping SHARED ${sources})
set_target_properties( libWarping PROPERTIES PREFIX "")

find_library(          Commons NAMES Commons HINTS "$ENV{HOME}/lib" required)
FIND_LIBRARY(          M_LIB m)
SET(                   EXTRA_LIBS ${M_LIB} "-lpthread")
include_directories(   "$ENV{HOME}/include")
target_link_libraries( libWarping ${EXTRA_LIBS} ${Commons})

string(TIMESTAMP COMPIL "%Y-%m-%d %H:%M:%S")
add_definitions(-DCOMPIL=" ${COMPIL}")

add_executable(        warping src/main.c)
target_link_libraries( warping libWarping ${Commons})

install(               TARGETS warping RUNTIME  DESTINATION "bin")
install(               TARGETS libWarping LIBRARY DESTINATION "lib")
install(               FILES   ${headers} DESTINATION "include")
set_target_properties( warping PROPERTIES INSTALL_RPATH "/usr/local/lib")
