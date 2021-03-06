project(screensaver.pingpong)

cmake_minimum_required(VERSION 3.0)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${PROJECT_SOURCE_DIR})

enable_language(CXX)

find_package(Kodi REQUIRED)

if(NOT WIN32)
  find_package(glm REQUIRED)

  if(APP_RENDER_SYSTEM STREQUAL "gl" OR NOT APP_RENDER_SYSTEM)
    find_package(OpenGl REQUIRED)
    set(DEPLIBS ${OPENGL_LIBRARIES})
    set(includes ${OPENGL_INCLUDE_DIR})
    add_definitions(${OPENGL_DEFINITIONS})
  elseif(APP_RENDER_SYSTEM STREQUAL "gles")
    find_package(OpenGLES REQUIRED)
    set(DEPLIBS ${OPENGLES_LIBRARIES})
    set(includes ${OPENGLES_INCLUDE_DIR})
    add_definitions(${OPENGLES_DEFINITIONS})
  endif()
endif()

include_directories(${GLM_INCLUDE_DIR}
                    ${KODI_INCLUDE_DIR}/.. # Hack way with "/..", need bigger Kodi cmake rework to match right include ways
                    ${PROJECT_SOURCE_DIR}/lib)

set(PINGPONG_SOURCES src/main.cpp
                     src/pingpong.cpp)

set(PINGPONG_HEADERS src/main.h
                     src/pingpong.h
                     src/timer.h
                     src/types.h)

build_addon(screensaver.pingpong PINGPONG DEPLIBS)

include(CPack)
