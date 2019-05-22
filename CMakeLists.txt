cmake_minimum_required(VERSION 3.12)

project(Dr-QP-docker C)

macro(docker_partials RESULT_VAR)
  cmake_parse_arguments(
    PARSED_ARGS 1
    ""
    "PARTS"
    ""
    ${ARGN})
  list(TRANSFORM PARSED_ARGS_PARTS
    PREPEND ${CMAKE_CURRENT_LIST_DIR}/partials/Dockerfile.
    OUTPUT_VARIABLE ${RESULT_VAR})
  
endmacro()

docker_partials(MAIN_DOCKER PARTS
  cuda
  miniconda
  python
  tensorflow-gpu
  jupyterlab
  code-server
  code-plugins
  main
)

message(STATUS ${MAIN_DOCKER})

add_custom_command( 
  OUTPUT Dockerfile
  DEPENDS ${MAIN_DOCKER}
  COMMENT "Merging main Dockerfile"
  COMMAND cat "$<JOIN:${MAIN_DOCKER},;>" > Dockerfile
  VERBATIM
  COMMAND_EXPAND_LISTS
)

set(DOCKER_TAG "drqp/dev-studio:latest")

add_custom_target( 
  Docker ALL 
  DEPENDS Dockerfile 
  COMMENT "Building Dockerfile"
  # COMMAND cat Dockerfile
  COMMAND docker build ${CMAKE_CURRENT_LIST_DIR}/context -f Dockerfile -t ${DOCKER_TAG}
  USES_TERMINAL
)

add_custom_target( 
  Upload
  DEPENDS Docker 
  COMMENT "Uploading docker images"
  COMMAND docker push ${DOCKER_TAG}
  USES_TERMINAL
)


add_custom_target(Purge
  COMMAND docker system prune --force
  COMMENT "Purging unused Docker images"
)

add_custom_target(PurgeAll
  COMMAND docker system prune --all --force
  COMMENT "Purging ALL Docker images, even used"
)
