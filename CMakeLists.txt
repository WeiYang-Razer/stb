cmake_minimum_required(VERSION 3.19)

file(READ "${CMAKE_CURRENT_SOURCE_DIR}/wyvrn.json" WYVRN_JSON)
string(JSON WYVRN_PROJECT_VERSION GET "${WYVRN_JSON}" version)

project(stb VERSION ${WYVRN_PROJECT_VERSION} LANGUAGES C)

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

file(GLOB STB_HEADERS "${CMAKE_CURRENT_SOURCE_DIR}/stb_*.h")

add_library(stb INTERFACE)
add_library(stb::stb ALIAS stb)

target_include_directories(stb INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

install(FILES ${STB_HEADERS}
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

install(TARGETS stb EXPORT stbTargets)

install(EXPORT stbTargets
    FILE stbTargets.cmake
    NAMESPACE stb::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/stb)

file(WRITE "${CMAKE_CURRENT_BINARY_DIR}/stbConfig.cmake"
    "include(\"\${CMAKE_CURRENT_LIST_DIR}/stbTargets.cmake\")\n")

write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/stbConfigVersion.cmake"
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion)

install(FILES
    "${CMAKE_CURRENT_BINARY_DIR}/stbConfig.cmake"
    "${CMAKE_CURRENT_BINARY_DIR}/stbConfigVersion.cmake"
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/stb)
