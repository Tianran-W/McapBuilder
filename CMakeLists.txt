# =================================================================================================
#  Copyright (C) 2023-2025 MCAP_BUILDER Contributors
# =================================================================================================

cmake_minimum_required(VERSION 3.22.1)

set(VERSION "1.4.1")

project(mcap_builder LANGUAGES CXX VERSION ${VERSION})

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(MCAP_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/third-party/mcap/cpp/mcap/include/)

add_subdirectory(src)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${PROJECT_BINARY_DIR}/mcap-config-version.cmake"
    VERSION ${CMAKE_PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion
)

install(TARGETS mcap
    EXPORT mcap-targets
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    INCLUDES
    DESTINATION include
)
install(DIRECTORY ${MCAP_INCLUDE_DIR} DESTINATION include)

include(CMakePackageConfigHelpers)
configure_package_config_file(
    "${PROJECT_SOURCE_DIR}/cmake/mcap-config.cmake.in"
    "${PROJECT_BINARY_DIR}/mcap-config.cmake"
    INSTALL_DESTINATION lib/cmake/mcap
)

install(EXPORT mcap-targets DESTINATION lib/cmake/mcap)
install(FILES "${PROJECT_BINARY_DIR}/mcap-config-version.cmake"
              "${PROJECT_BINARY_DIR}/mcap-config.cmake"
        DESTINATION lib/cmake/mcap)
