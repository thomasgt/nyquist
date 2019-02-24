# configure version header
configure_file(version.in.hpp ${CMAKE_CURRENT_BINARY_DIR}/version.hpp)

# generate compile proto files
set(PROTOS channel.proto
    common.proto
    node.proto)

protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS ${PROTOS})
protobuf_generate_cpp(PROTO_GRPC_SRCS PROTO_GRPC_HDRS ${PROTOS})

set(SRCS)
set(HDRS)

add_library(nyquistcore ${SRCS} ${PROTO_SRCS} ${PROTO_GRPC_SRCS})

if (WIN32)
    target_compile_definitions(nyquistcore PRIVATE "_WIN32_WINNT=0x0A00")
endif()


target_link_libraries(nyquistcore PUBLIC gRPC::grpc++)
target_link_libraries(nyquistcore PUBLIC protobuf::libprotobuf)

target_include_directories(nyquistcore
                           PUBLIC
                           "$<INSTALL_INTERFACE:include>"
                           "$<BUILD_INTERFACE:${PROJECT_BINARY_DIR}>"
                           "$<BUILD_INTERFACE:${PROJECT_DIR}>")

set_target_properties(nyquistcore PROPERTIES PUBLIC_HEADER ${HDRS} ${PROTO_HDRS} ${PROTO_GRPC_HDRS})
add_library(nyquist::core ALIAS nyquistcore)


