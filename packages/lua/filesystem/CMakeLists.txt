cmake_minimum_required(VERSION 3.16)

project(LuaFileSystem
    LANGUAGES C)

find_package(DeLua 5.4 REQUIRED)
set(CMAKE_INSTALL_PREFIX ${DeLua_PREFIX} CACHE PATH "Installation prefix (from DeLua)." FORCE)
DeLua_PROGDIR(DeLua_CACHEDIR "cache/lua")

include(ExternalProject)

ExternalProject_Add(luafilesystem
    PREFIX "${DeLua_CACHEDIR}"
    GIT_REPOSITORY "https://github.com/lunarmodules/luafilesystem"
    GIT_PROGRESS ON
    CONFIGURE_COMMAND ""
    BUILD_COMMAND ""
    INSTALL_COMMAND ""
) 

set(luafilesystem_SOURCE_DIR ${DeLua_CACHEDIR}/src/luafilesystem)

add_library(lfs MODULE
    ${luafilesystem_SOURCE_DIR}/src/lfs.h
    ${luafilesystem_SOURCE_DIR}/src/lfs.c
)
add_dependencies(lfs luafilesystem)
set_target_properties(lfs
    PROPERTIES
        PREFIX ""
        OUTPUT_NAME lfs)
target_link_libraries(lfs DeLua::Library::C) 

install(TARGETS lfs 
    LIBRARY DESTINATION ${DeLua_CPATH_DIR} COMPONENT "runtime")

install(DIRECTORY ${luafilesystem_SOURCE_DIR}/docs
    DESTINATION ${DeLua_DOCDIR}/luafilesystem COMPONENT "documentation")
