cmake_minimum_required(VERSION 3.0)
project(Tunnel)
set(CMAKE_CXX_STANDARD 20)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}../../bin)

if (APPLE)
    message(DEBUG "[OS] Apple")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -lpthread")
elseif (UNIX)
    message(DEBUG "[OS] Linux")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -luuid")
endif()

add_executable(server
        server/main.cpp
        server/config.hpp
        server/config.cpp
        server/connection.cpp
        server/connection.hpp
        server/message.cpp
        server/message.hpp
        server/shared.cpp
        server/shared.hpp)
add_executable(client
        client/main.cpp
        client/message.cpp
        client/message.hpp
        client/config.cpp
        client/config.hpp
        client/connection.cpp
        client/connection.hpp
        client/shared.cpp
        client/shared.hpp
        client/opt.cpp
        client/opt.hpp)

if (UNIX AND NOT APPLE)
    find_library(UUID_LIB uuid REQUIRED)
    target_link_libraries(server PRIVATE ${UUID_LIB})
endif()
