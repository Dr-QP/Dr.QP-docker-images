cmake_minimum_required(VERSION 3.12)

project(Dr-QP-docker C)
include(ExternalProject)


add_custom_command( 
  OUTPUT Dockerfile
  DEPENDS 
    Dockerfile.main
    partials/Dockerfile.cuda
    partials/Dockerfile.miniconda
    partials/Dockerfile.tensorflow-gpu
    partials/Dockerfile.jupyterlab
    partials/Dockerfile.python
    partials/Dockerfile.code-server
  COMMENT "Preprocessing Dockerfile"
  COMMAND "${CMAKE_C_COMPILER}" 
    --language c
    --preprocess "${CMAKE_CURRENT_LIST_DIR}/Dockerfile.main"
    --include-directory ${CMAKE_CURRENT_LIST_DIR}/partials
    | grep -v "#" > Dockerfile
  VERBATIM
)

set(DOCKER_TAG "drqp/dev-studio:latest")

add_custom_target( 
  Docker ALL 
  DEPENDS Dockerfile 
  COMMENT "Building Dockerfile"
  COMMAND cat Dockerfile
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
