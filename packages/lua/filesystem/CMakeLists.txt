cmake_minimum_required(VERSION 3.16)

include(FetchContent)
FetchContent_Declare(luafilesystem
    PREFIX "${DeLua_CACHEDIR}"
    GIT_REPOSITORY "https://github.com/lunarmodules/luafilesystem"
    GIT_PROGRESS ON
) 
FetchContent_MakeAvailable(luafilesystem)

project(LuaFileSystem
    LANGUAGES C)

find_package(DeLua 5.4 REQUIRED)
set(CMAKE_INSTALL_PREFIX ${DeLua_PREFIX} CACHE PATH "Installation prefix (from DeLua)." FORCE)
DeLua_PROGDIR(DeLua_CACHEDIR "cache/lua")

add_library(lfs MODULE)
set_target_properties(lfs
    PROPERTIES
        PREFIX ""
        OUTPUT_NAME lfs)
target_link_libraries(lfs DeLua::Library::C) 
target_sources(lfs
    PRIVATE
        ${luafilesystem_SOURCE_DIR}/src/lfs.h
        ${luafilesystem_SOURCE_DIR}/src/lfs.c
)

install(TARGETS lfs 
    LIBRARY DESTINATION ${DeLua_CPATH_DIR} COMPONENT "runtime")

install(DIRECTORY ${luafilesystem_SOURCE_DIR}/docs
    DESTINATION ${DeLua_DOCDIR}/luafilesystem COMPONENT "documentation")
