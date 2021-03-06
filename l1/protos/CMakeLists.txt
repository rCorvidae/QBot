cmake_minimum_required(VERSION 3.5 FATAL_ERROR)

project(protobuf_service)

if (NOT "${CMAKE_CURRENT_SOURCE_DIR}/service.pb.h")
    execute_process(
        COMMAND /bin/sh ${CMAKE_CURRENT_SOURCE_DIR}/generate.sh
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
endif (NOT "${CMAKE_CURRENT_SOURCE_DIR}/service.pb.h")

add_library(protobuf_svc
    "service.pb.h"
    "service.pb.cc"
    "service.grpc.pb.h"
    "service.grpc.pb.cc"
    )

find_package(Protobuf REQUIRED)
include_directories(${Protobuf_INCLUDE_DIRS})
include_directories(${CMAKE_CURRENT_BINARY_DIR})

find_package(PkgConfig REQUIRED)
pkg_check_modules(GRPC REQUIRED grpc grpc++)
target_link_libraries(protobuf_svc ${GRPC_LIBRARIES})
#target_link_options(protobuf_svc PUBLIC -lgrpc++_reflection -ldl)
target_include_directories(protobuf_svc PUBLIC ${GRPC_INCLUDE_DIRS})
