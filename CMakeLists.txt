cmake_minimum_required(VERSION 3.10)

project(nyquist VERSION 0.0.0 LANGUAGES CXX)

find_package(Protobuf CONFIG REQUIRED)
find_package(gRPC CONFIG REQUIRED)

# get the protobuf creation functions
include(cmake/proto-compile.cmake REQUIRED)

add_subdirectory(nyquist)
