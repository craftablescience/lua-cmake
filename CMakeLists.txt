cmake_minimum_required(VERSION 3.16)
project(lua LANGUAGES C)
set(CMAKE_C_STANDARD 11)

include(FetchContent)
FetchContent_Declare(
        ${PROJECT_NAME}
        GIT_REPOSITORY "https://github.com/lua/lua"
        GIT_TAG "master"
        SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/lua")
FetchContent_MakeAvailable(${PROJECT_NAME})

file(GLOB LUA_SOURCES "${lua_SOURCE_DIR}/*.c" "${lua_SOURCE_DIR}/*.h")
list(FILTER LUA_SOURCES EXCLUDE REGEX ".*onelua.cpp$")
add_library(${PROJECT_NAME} STATIC ${LUA_SOURCES})

target_compile_definitions(${PROJECT_NAME}
        PRIVATE
        $<$<PLATFORM_ID:Linux>:LUA_USE_LINUX LUA_COMPAT_5_2>)

target_compile_options(${PROJECT_NAME}
        PRIVATE
        $<$<OR:$<C_COMPILER_ID:AppleClang>,$<C_COMPILER_ID:Clang>,$<C_COMPILER_ID:GNU>>:
        -Wextra -Wshadow -Wsign-compare -Wundef -Wwrite-strings -Wredundant-decls
        -Wdisabled-optimization -Waggregate-return -Wdouble-promotion -Wdeclaration-after-statement
        -Wmissing-prototypes -Wnested-externs -Wstrict-prototypes -Wc++-compat -Wold-style-definition>)

target_include_directories(${PROJECT_NAME}
        PUBLIC
        "${lua_SOURCE_DIR}")

add_library(lua::lua ALIAS lua)
