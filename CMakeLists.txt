cmake_minimum_required(VERSION 3.23)
project(ImPlot VERSION 0.17)

include(GNUInstallDirs)

find_package(imgui REQUIRED)

add_library(implot)
target_sources(implot
    PUBLIC
        FILE_SET HEADERS
        FILES implot.h implot_internal.h
    PRIVATE
        implot.cpp implot_items.cpp
)
target_link_libraries(implot PUBLIC imgui::imgui imgui::backend_glfw_vulkan)

install(TARGETS implot
        EXPORT implotTargets
        ARCHIVE DESTINATION lib
        FILE_SET HEADERS DESTINATION include
)

install(EXPORT implotTargets
        NAMESPACE implot::
        DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/cmake/implot
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
