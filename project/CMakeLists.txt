cmake_minimum_required(VERSION 3.16.0)

if(NOT CMAKE_VERSION VERSION_LESS 3.0)
    cmake_policy(SET CMP0048 NEW)
endif()

project(DDSHelloWorld)

# Find requirements
# if(NOT fastcdr_FOUND)
#     find_package(fastcdr REQUIRED)
# endif()

# if(NOT fastrtps_FOUND)
#     find_package(fastrtps REQUIRED)
# endif()



# add_library(fast_dds_lib Fast-DDS/include/libfastcdr.so
# Fast-DDS/include/libfastcdr.so.1
# Fast-DDS/include/libfastcdr.so.1.0.21
# Fast-DDS/lib/libfastrtps.so
# Fast-DDS/lib/libfastrtps.so.2.3
# Fast-DDS/lib/libfastrtps.so.2.3.3

# )   

add_library(fastcdr SHARED ${CMAKE_SOURCE_DIR}/Fast-DDS/include/fastcdr/Cdr.h
${CMAKE_SOURCE_DIR}/Fast-DDS/include/fastcdr/config.h
${CMAKE_SOURCE_DIR}/Fast-DDS/include/fastcdr/eProsima_auto_link.h
${CMAKE_SOURCE_DIR}/Fast-DDS/include/fastcdr/FastBuffer.h
${CMAKE_SOURCE_DIR}/Fast-DDS/include/fastcdr/fastcdr_dll.h
${CMAKE_SOURCE_DIR}/Fast-DDS/include/fastcdr/FastCdr.h
${CMAKE_SOURCE_DIR}/Fast-DDS/include/fastcdr/exceptions/BadParamException.h
${CMAKE_SOURCE_DIR}/Fast-DDS/include/fastcdr/exceptions/Exception.h
${CMAKE_SOURCE_DIR}/Fast-DDS/include/fastcdr/exceptions/NotEnoughMemoryException.h
${CMAKE_SOURCE_DIR}/Fast-DDS/lib/libfastcdr.so
${CMAKE_SOURCE_DIR}/Fast-DDS/lib/libfastcdr.so.1
${CMAKE_SOURCE_DIR}/Fast-DDS/lib/libfastcdr.so.1.0.21
)



add_library(fastrtps SHARED ${CMAKE_SOURCE_DIR}/Fast-DDS/include/fastrtps/config.h
${CMAKE_SOURCE_DIR}/Fast-DDS/include/fastrtps/Domain.h
${CMAKE_SOURCE_DIR}/Fast-DDS/include/fastrtps/eProsima_auto_link.h
${CMAKE_SOURCE_DIR}/Fast-DDS/include/fastrtps/fastrtps_all.h
${CMAKE_SOURCE_DIR}/Fast-DDS/include/fastrtps/fastrtps_dll.h
${CMAKE_SOURCE_DIR}/Fast-DDS/include/fastrtps/fastrtps_fwd.h
${CMAKE_SOURCE_DIR}/Fast-DDS/include/fastrtps/TopicDataType.h
)

# add_library(fastrtps ${CMAKE_SOURCE_DIR}/Fast-DDS/include/fastrtps/config.h
# Fast-DDS/lib/libfastrtps.so
# Fast-DDS/lib/libfastrtps.so.2.3
# Fast-DDS/lib/libfastrtps.so.2.3.3)

set_target_properties(fastcdr PROPERTIES LINKER_LANGUAGE CXX)
set_target_properties(fastrtps PROPERTIES LINKER_LANGUAGE CXX)

# target_link_directories(fastrtps PUBLIC ${CMAKE_SOURCE_DIR}/Fast-DDS/include)
target_include_directories(fastrtps PUBLIC ${CMAKE_SOURCE_DIR}/Fast-DDS/include)




message(STATUS "fastcdr_LIB_DIR = ${fastcdr_LIB_DIR}")
message(STATUS "fastcdr_INCLUDE_DIR = ${fastcdr_INCLUDE_DIR}")

message(STATUS "fastrtps_LIB_DIR = ${fastrtps_LIB_DIR}")
message(STATUS "fastrtps_INCLUDE_DIR = ${fastcdr_INCLUDE_DIR}")


message(STATUS "CMAKE_SOURCE_DIR = ${CMAKE_SOURCE_DIR}")
message(STATUS "CMAKE_SOURCE_DIR = ${CMAKE_SOURCE_DIR}/Fast-DDS")
message(STATUS "GLOB sources = ${DDS_HELLOWORLD_SOURCES_CXX}")

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
# add_executable(DDSHelloWorldSubscriber HelloWorldSubscriber.cpp ${DDS_HELLOWORLD_SOURCES_CXX})
# target_link_libraries(DDSHelloWorldSubscriber fastrtps fastcdr)






# get_cmake_property(_variableNames VARIABLES)
# list (SORT _variableNames)
# foreach (_variableName ${_variableNames})
#     message(STATUS "${_variableName}=${${_variableName}}")
# endforeach()