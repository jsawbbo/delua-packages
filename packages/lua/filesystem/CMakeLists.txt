cmake_minimum_required(VERSION 3.16)

project(LuaFileSystem
    LANGUAGES C)

find_package(DeLua 5.4 REQUIRED)

include(FetchContent)
FetchContent_Populate(luafilesystem
    GIT_REPOSITORY "https://github.com/lunarmodules/luafilesystem"
    GIT_PROGRESS ON
    SOURCE_DIR "${PAM_CACHEDIR}/luafilesystem"
) 

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
    LIBRARY DESTINATION ${DeLua_CDIR} COMPONENT "runtime")

install(DIRECTORY ${PAM_CACHEDIR}/luafilesystem/docs
    DESTINATION ${DeLua_DOCDIR}/luafilesystem COMPONENT "documentation")
