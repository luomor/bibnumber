cmake_minimum_required (VERSION 3.2)
project (BIBNUMBER)

include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
        message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

set(CMAKE_BUILD_TYPE Debug)

find_package(OpenCV REQUIRED core imgproc video highgui features2d)

################ Boost #################
set(Boost_USE_STATIC_LIBS        ON)
FIND_PACKAGE(Boost COMPONENTS system REQUIRED)
include_directories(${Boost_INCLUDE_DIR})
########################################

file(GLOB BIBNUMBER_SOURCES bibnumber/*.cpp bibnumber/*.c)
file(GLOB BIBNUMBER_HEADERS bibnumber/*.hpp bibnumber/*.h)

add_executable(bibnumber
    ${BIBNUMBER_SOURCES}
    ${BIBNUMBER_HEADERS}
    )

target_link_libraries(bibnumber
    ${Boost}
    ${OpenCV_LIBS}
)