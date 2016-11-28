cmake_minimum_required(VERSION 3.5)
cmake_policy(SET CMP0015 NEW)
cmake_policy(SET CMP0003 NEW)

ENABLE_LANGUAGE(C)
project(gbdxm CXX)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -fPIC")

# gbdxm version
set(gbdxm_VERSION_MAJOR 0)
set(gbdxm_VERSION_MINOR 1)

# Set the build number
if(DEFINED ENV{BUILD_NUMBER})
    set(gbdxm_VERSION_BUILD $ENV{BUILD_NUMBER})
else()
    set(gbdxm_VERSION_BUILD "SNAPSHOT")
endif()

# Initialize gbdxversion.h
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/gbdxmVersion.h.in ${CMAKE_BINARY_DIR}/include/gbdxmVersion.h)
include_directories(${CMAKE_BINARY_DIR}/include)

add_executable(gbdxm
        src/main.cpp
        src/gbdxm.h
        src/gbdxm.cpp)

find_package(DeepCore REQUIRED)
if (DeepCore_FOUND)
    include_directories(${DEEPCORE_INCLUDE_DIRS})
    target_link_libraries(gbdxm ${DEEPCORE_LIBRARIES})
    list(APPEND CMAKE_MODULE_PATH ${DEEPCORE_CMAKE_DIR})
endif()

find_package(Minizip REQUIRED)
if(MINIZIP_FOUND)
    include_directories(${MINIZIP_INCLUDE_DIRS})
    target_link_libraries(gbdxm ${MINIZIP_LIBRARIES})
endif()

find_package(ZLIB REQUIRED)
if(ZLIB_FOUND)
    include_directories(${ZLIB_INCLUDE_DIRS})
    target_link_libraries(gbdxm ${ZLIB_LIBRARIES})
endif()

set(Boost_USE_STATIC_LIBS ON)
set(Boost_USE_MULTITHREADED ON)
find_package(Boost COMPONENTS program_options REQUIRED)
if(Boost_FOUND)
    target_link_libraries(gbdxm ${Boost_PROGRAM_OPTIONS_LIBRARY})
endif()