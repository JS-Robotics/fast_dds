cmake_minimum_required(VERSION 3.16.0)

if(NOT CMAKE_VERSION VERSION_LESS 3.0)
    cmake_policy(SET CMP0048 NEW)
endif()

project(DDSHelloWorld)

# Find requirements
if(NOT fastcdr_FOUND)
    find_package(fastcdr REQUIRED)
endif()

if(NOT fastrtps_FOUND)
    find_package(fastrtps REQUIRED)
endif()

# add_library(fast_dds_lib Fast-DDS/include/libfastcdr.so
# Fast-DDS/include/libfastcdr.so.1
# Fast-DDS/include/libfastcdr.so.1.0.21
# Fast-DDS/lib/libfastrtps.so
# Fast-DDS/lib/libfastrtps.so.2.3
# Fast-DDS/lib/libfastrtps.so.2.3.3

# )       


message(STATUS "fastcdr_LIB_DIR = ${fastcdr_LIB_DIR}")
message(STATUS "fastcdr_INCLUDE_DIR = ${fastcdr_INCLUDE_DIR}")

message(STATUS "fastrtps_LIB_DIR = ${fastrtps_LIB_DIR}")
message(STATUS "fastrtps_INCLUDE_DIR = ${fastcdr_INCLUDE_DIR}")

# Set C++11
include(CheckCXXCompilerFlag)
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_COMPILER_IS_CLANG OR
        CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    check_cxx_compiler_flag(-std=c++11 SUPPORTS_CXX11)
    if(SUPPORTS_CXX11)
        add_compile_options(-std=c++11)
    else()
        message(FATAL_ERROR "Compiler doesn't support C++11")
    endif()
endif()

message(STATUS "Configuring HelloWorld publisher/subscriber example...")
# file(GLOB DDS_HELLOWORLD_SOURCES_CXX "src/*.cxx")
file(GLOB DDS_HELLOWORLD_SOURCES_CXX "*.cxx")

# add_executable(DDSHelloWorldPublisher src/HelloWorldPublisher.cpp ${DDS_HELLOWORLD_SOURCES_CXX})
add_executable(DDSHelloWorldPublisher HelloWorldPublisher.cpp ${DDS_HELLOWORLD_SOURCES_CXX})
target_link_libraries(DDSHelloWorldPublisher fastrtps fastcdr)

# add_executable(DDSHelloWorldSubscriber src/HelloWorldSubscriber.cpp ${DDS_HELLOWORLD_SOURCES_CXX})
add_executable(DDSHelloWorldSubscriber HelloWorldSubscriber.cpp ${DDS_HELLOWORLD_SOURCES_CXX})
target_link_libraries(DDSHelloWorldSubscriber fastrtps fastcdr)