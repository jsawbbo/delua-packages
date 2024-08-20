cmake_minimum_required(VERSION 3.16)

include(FetchContent)
FetchContent_Declare(luafilesystem
    PREFIX "${PAM_CACHEDIR}"
    GIT_REPOSITORY "https://github.com/lunarmodules/luafilesystem"
    GIT_PROGRESS ON
) 
FetchContent_MakeAvailable(luafilesystem)

project(LuaFileSystem
    LANGUAGES C)

find_package(DeLua 5.4 REQUIRED)

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
