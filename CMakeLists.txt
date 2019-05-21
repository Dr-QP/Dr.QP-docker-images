cmake_minimum_required(VERSION 3.12)

set(CMAKE_VERBOSE_MAKEFILE ON CACHE BOOL "ON")

project(Dr-QP-docker C)
include(ExternalProject)

add_subdirectory(partials)

add_custom_command( 
  OUTPUT Dockerfile
  DEPENDS 
    partials/Dockerfile.cuda
    partials/Dockerfile.miniconda
    partials/Dockerfile.tensorflow-gpu
  COMMENT "Preprocessing Dockerfile"
  COMMAND "${CMAKE_C_COMPILER}" 
    --language c
    --preprocess "${CMAKE_CURRENT_LIST_DIR}/Dockerfile.main"
    --include-directory ${CMAKE_CURRENT_LIST_DIR}/partials
    | grep -v "#" > Dockerfile
  VERBATIM
)

add_custom_target( 
  Docker ALL 
  DEPENDS Dockerfile 
  COMMENT "Building Dockerfile"
  COMMAND docker build ${CMAKE_CURRENT_LIST_DIR}/context -f Dockerfile -t dr-qp/dev-studio:latest
  USES_TERMINAL
) 