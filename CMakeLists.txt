cmake_minimum_required(VERSION 3.13)

project(Dr-QP-docker C)
include(ExternalProject)

add_subdirectory(partials)

# add_custom_command( 
#   OUTPUT Dockerfile
#   DEPENDS ${PARTIALS}
#   COMMAND "${CMAKE_C_COMPILER}" -E Dockerfile.main -I partials
#   COMMENT "Preprocessing Dockerfile.main" 
#   VERBATIM 
# ) 
include_directories(partials)
add_library(Dockerfile EXCLUDE_FROM_ALL Dockerfile.main.c)


# add_custom_target( 
#   ProcessSQL ALL 
#   DEPENDS myfile.sql 
#   COMMENT "Preprocessing SQL files" 
#   VERBATIM 
# ) 