cmake_minimum_required(VERSION 3.16)

include(../../delua.cmake)

project(DBI
    LANGUAGES C)

pam_module(dbi
    VERSION "scm"
    SOURCE "git+https://github.com/mwild1/luadbi.git"
    
    DESCRIPTION 
        SUMMARY "Database abstraction layer"
        DETAILED "
            LuaDBI is a database interface library for Lua. It is designed
            to provide a RDBMS agnostic API for handling database
            operations. LuaDBI also provides support for prepared statement
            handles, placeholders and bind parameters for all database
            operations."
        LICENSE "MIT/X11"
        HOMEPAGE "https://github.com/mwild1/luadbi"

    DEPENDS
        "lua >= 5.1"
    
    MODULE DBI
        SCRIPTS
            DBI.lua
)
