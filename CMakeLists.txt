cmake_minimum_required(VERSION 3.0 FATAL_ERROR)

# check the original project at:
# https://github.com/LoopPerfect/neither

project(neither
    VERSION 0.5.0.0
    DESCRIPTION "Either and Maybe monads for better error-handling in C++"
    LANGUAGES CXX
    )

set(neither_INCLUDES
    include/neither/neither.hpp
    include/neither/either.hpp
    include/neither/maybe.hpp
    include/neither/traits.hpp
    include/neither/lift.hpp
    include/neither/try.hpp
    )

add_library(${PROJECT_NAME} INTERFACE )

target_include_directories(${PROJECT_NAME}
    INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
    )

install(TARGETS neither EXPORT neitherTargets
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
    )

install(
    FILES ${neither_INCLUDES}
    DESTINATION "include/neither"
    COMPONENT Devel
    )

include(CMakePackageConfigHelpers)

write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/neither/neither-config-version.cmake"
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion
    )

export(EXPORT neitherTargets
    FILE "${CMAKE_CURRENT_BINARY_DIR}/neither/neither-targets.cmake"
    )

configure_file(cmake/neither-config.cmake
    "${CMAKE_CURRENT_BINARY_DIR}/neither/neither-config.cmake"
    COPYONLY
    )

set(ConfigPackageLocation lib/cmake/neither)

install(EXPORT neitherTargets
    FILE "neither-targets.cmake"
    DESTINATION ${ConfigPackageLocation}
    )

install(
    FILES "cmake/neither-config.cmake" "${CMAKE_CURRENT_BINARY_DIR}/neither/neither-config-version.cmake"
    DESTINATION ${ConfigPackageLocation}
    COMPONENT Devel
)


set(CPACK_PACKAGE_VERSION_MAJOR ${PROJECT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR ${PROJECT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH ${PROJECT_VERSION_PATCH})
set(CPACK_PACKAGE_DESCRIPTION "A functional implementation of Either in C++14")
set(CPACK_PACKAGE_CONTACT "Rui Maciel rui.maciel@gmail.com")

include(CPack)
