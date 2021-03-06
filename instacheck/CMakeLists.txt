cmake_minimum_required(VERSION 3.10)
project(instacheck VERSION 0.0.1)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)
set(CMAKE_BUILD_TYPE "Release")
enable_testing()

if (UNIX)
    include_directories(SYSTEM "/usr/local/include")
    link_directories(SYSTEM "/usr/local/lib")
endif ()

if (MSVC)
    include_directories(SYSTEM ${CMAKE_INSTALL_PREFIX}/../xenon/include)
    link_directories(SYSTEM ${CMAKE_INSTALL_PREFIX}/../xenon/lib)
endif ()

add_executable(decode decode.cpp)

if (MSVC)
    target_link_libraries(decode xenon)
    add_test(NAME decode COMMAND decode
        WORKING_DIRECTORY ${CMAKE_INSTALL_PREFIX}/../xenon/share/xddl)
else ()
    target_link_libraries(decode -lxenon)
    add_test(NAME decode COMMAND decode
        WORKING_DIRECTORY ${CMAKE_INSTALL_PREFIX}/share/xddl)
endif ()
