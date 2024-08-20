cmake_minimum_required(VERSION 3.16)

project(LuaFileSystem
    LANGUAGES C)

include(../../pam.cmake)

FetchContent_Populate(luafilesystem
    GIT_REPOSITORY "https://github.com/lunarmodules/luafilesystem"
    GIT_PROGRESS ON
    SOURCE_DIR "${PAM_CACHEDIR}/luafilesystem"
) 

# delua_module(luafilesystem
#     VERSION "scm"
#     SOURCE GIT "https://github.com/lunarmodules/luafilesystem"
#     SUMMARY "File System Library for the Lua Programming Language"
#     DESCRIPTION "
#         LuaFileSystem is a Lua library developed to complement the set of
#         functions related to file systems offered by the standard Lua
#         distribution. LuaFileSystem offers a portable way to access the
#         underlying directory structure and file attributes."
#     LICENSE "MIT/X11"
#     DEPENDS
#         "lua >= 5.1"
#     MODULE lfs
#         SOURCES 
#             src/lfs.h
#             src/lfs.c
#     COMPONENT "docs"
#         DIRECTORY ${PAM_CACHEDIR}/luafilesystem/docs
#         DESTINATION ${DeLua_DOCDIR}/luafilesystem
#     COMPONENT "tests"
#         DIRECTORY ${PAM_CACHEDIR}/luafilesystem/tests
#         DESTINATION ${DeLua_DOCDIR}/luafilesystem
# )

add_library(lfs MODULE)
set_target_properties(lfs
    PROPERTIES
        PREFIX ""
        OUTPUT_NAME lfs)
target_link_libraries(lfs DeLua::Library::C) 
target_sources(lfs
    PRIVATE
        ${PAM_CACHEDIR}/luafilesystem/src/lfs.h
        ${PAM_CACHEDIR}/luafilesystem/src/lfs.c
)

install(TARGETS lfs 
    LIBRARY DESTINATION ${LUA_CDIR} COMPONENT "runtime")

install(DIRECTORY ${PAM_CACHEDIR}/luafilesystem/docs
    DESTINATION ${DeLua_DOCDIR}/luafilesystem COMPONENT "documentation")
