cmake_minimum_required(VERSION 3.9)

set(CMAKE_BUILD_TYPE Release)

project(lua-emscripten)

file(GLOB_RECURSE SOURCES *.c)
file(GLOB_RECURSE HEADERS *.h)

add_library(${PROJECT_NAME} STATIC ${SOURCES})

include(GNUInstallDirs)

include(CMakePackageConfigHelpers)

target_include_directories(${PROJECT_NAME} PUBLIC
        $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}>
        $<INSTALL_INTERFACE:include/lua>)

install(TARGETS
        ${PROJECT_NAME}
        EXPORT lua-emscripten-targets
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
        )

install(EXPORT ${PROJECT_NAME}-targets
        NAMESPACE lua::
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
        )

install(FILES
        ${HEADERS}
        DESTINATION
        ${CMAKE_INSTALL_INCLUDEDIR}/lua
        )

configure_package_config_file(
        "${PROJECT_SOURCE_DIR}/cmake/lua-emscripten-config.cmake.in"
        "${PROJECT_BINARY_DIR}/cmake/lua-emscripten-config.cmake"
        INSTALL_DESTINATION lib/cmake/lua-emscripten
)

install(FILES
        "${PROJECT_BINARY_DIR}/cmake/lua-emscripten-config.cmake"
        DESTINATION lib/cmake/lua-emscripten)