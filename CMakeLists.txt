cmake_minimum_required( VERSION 3.15 )

project( luacpp VERSION 1.0.0 )

add_library( luacpp INTERFACE )

target_include_directories( luacpp INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)
