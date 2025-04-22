cmake_minimum_required(VERSION 3.23)
project(ImPlot VERSION 0.17)

include(GNUInstallDirs)

find_package(imgui REQUIRED)

add_library(implot STATIC)
add_library(implot::implot ALIAS implot)
target_sources(implot
    PUBLIC
        FILE_SET HEADERS
        FILES implot.h implot_internal.h
    PRIVATE
        implot.cpp implot_items.cpp
)
target_link_libraries(implot PUBLIC imgui::imgui)

set_target_properties(implot PROPERTIES EXPORT_NAME "implot")
install(TARGETS implot
        EXPORT implotTargets
        ARCHIVE DESTINATION lib
        FILE_SET HEADERS DESTINATION include
)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/implotConfigVersion.cmake"
    COMPATIBILITY SameMajorVersion
)

install(FILES
    "${CMAKE_CURRENT_BINARY_DIR}/implotConfigVersion.cmake"
    DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/cmake/implot
)

configure_file(cmake/implotConfig.cmake.in implotConfig.cmake @ONLY)
install(FILES
    "${CMAKE_CURRENT_BINARY_DIR}/implotConfig.cmake"
    DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/cmake/implot
)

add_library(implot_demo STATIC)
add_library(implot::demo ALIAS implot_demo)
target_sources(implot_demo
    PRIVATE implot_demo.cpp
)
target_link_libraries(implot_demo PUBLIC implot::implot)

set_target_properties(implot_demo PROPERTIES EXPORT_NAME "demo")
install(TARGETS implot_demo
        EXPORT implotTargets
        ARCHIVE DESTINATION lib
)

install(EXPORT implotTargets
        NAMESPACE implot::
        DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/cmake/implot
)
