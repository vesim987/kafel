cmake_minimum_required(VERSION 2.8.12)

#project(kafel)

find_package(BISON REQUIRED)
find_package(FLEX REQUIRED)

include_directories(${CMAKE_CURRENT_BINARY_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src)

BISON_TARGET(MyParser ${CMAKE_CURRENT_SOURCE_DIR}/src/parser.y ${CMAKE_CURRENT_BINARY_DIR}/parser.c 
    DEFINES_FILE ${CMAKE_CURRENT_BINARY_DIR}/parser.h)
FLEX_TARGET(MyScanner ${CMAKE_CURRENT_SOURCE_DIR}/src/lexer.l ${CMAKE_CURRENT_BINARY_DIR}/lexer.c)
ADD_FLEX_BISON_DEPENDENCY(MyScanner MyParser)

add_library(kafel
    ${CMAKE_CURRENT_SOURCE_DIR}/src/codegen.c
    ${CMAKE_CURRENT_SOURCE_DIR}/src/context.c
    ${CMAKE_CURRENT_SOURCE_DIR}/src/expression.c
    ${CMAKE_CURRENT_SOURCE_DIR}/src/includes.c
    ${CMAKE_CURRENT_SOURCE_DIR}/src/kafel.c
    ${CMAKE_CURRENT_SOURCE_DIR}/src/policy.c 
    ${CMAKE_CURRENT_SOURCE_DIR}/src/range_rules.c
    ${CMAKE_CURRENT_SOURCE_DIR}/src/syscall.c
    ${CMAKE_CURRENT_SOURCE_DIR}/src/syscalls/aarch64_syscalls.c
    ${CMAKE_CURRENT_SOURCE_DIR}/src/syscalls/amd64_syscalls.c
    ${CMAKE_CURRENT_SOURCE_DIR}/src/syscalls/arm_syscalls.c
    ${CMAKE_CURRENT_SOURCE_DIR}/src/syscalls/i386_syscalls.c
    ${CMAKE_CURRENT_SOURCE_DIR}/src/syscalls/mips64_syscalls.c
    ${CMAKE_CURRENT_SOURCE_DIR}/src/syscalls/mipso32_syscalls.c
    ${BISON_MyParser_OUTPUTS} 
    ${FLEX_MyScanner_OUTPUTS} )

