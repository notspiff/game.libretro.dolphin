project(game.libretro.dolphin)

cmake_minimum_required(VERSION 2.6)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})

enable_language(CXX)

find_package(kodi REQUIRED)

include(ExternalProject)

if(CMAKE_BUILD_TYPE MATCHES Debug)
  set(LIBRETRO_DEBUG DEBUG=1)
endif()

if("${CORE_SYSTEM_NAME}" STREQUAL "windows")
  set(BUILD_COMMAND mingw32-make -C libretro platform=win ${LIBRETRO_DEBUG})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "linux")
  set(BUILD_COMMAND make -C libretro ${LIBRETRO_DEBUG})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "darwin")
  set(BUILD_COMMAND make -C libretro platform=osx ${LIBRETRO_DEBUG})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "ios")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "android")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "rbpi")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "freebsd")
# TODO
endif()

externalproject_add(dolphin
                    GIT_REPOSITORY https://github.com/libretro/dolphin
                    GIT_TAG 2ed82b9
                    PREFIX dolphin
                    CONFIGURE_COMMAND ""
                    INSTALL_COMMAND ""
                    BUILD_COMMAND ${BUILD_COMMAND}
                    BUILD_IN_SOURCE 1)

set(DOLPHIN_CUSTOM_BINARY ${PROJECT_BINARY_DIR}/dolphin/src/dolphin/libretro/dolphin_libretro${CMAKE_SHARED_LIBRARY_SUFFIX}
                          game.libretro.dolphin${CMAKE_SHARED_LIBRARY_SUFFIX} dolphin)

build_addon(game.libretro.dolphin DOLPHIN DEPLIBS)
add_dependencies(game.libretro.dolphin dolphin)

include(CPack)
