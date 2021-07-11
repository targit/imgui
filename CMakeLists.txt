project( imgui CXX )
cmake_minimum_required( VERSION 3.20 )

set( IMGUI_SRC_ROOT ${CMAKE_CURRENT_SOURCE_DIR})

set( CMAKE_CXX_STANDARD_REQUIRED TRUE )
set( CMAKE_CXX_STANDARD 17 )
set( CMAKE_POSITION_INDEPENDENT_CODE TRUE )

find_package( Threads REQUIRED )

set(
  INSTALL_TARGETS_DEFAULT_ARGS
  EXPORT imguiTargets
  ARCHIVE DESTINATION  "lib/"
  LIBRARY DESTINATION  "lib/"
  RUNTIME DESTINATION  "bin/"
  INCLUDES DESTINATION "include/")

add_library(
  base
  STATIC
  imgui.cpp
  imgui_draw.cpp
  imgui_tables.cpp
  imgui_widgets.cpp
  )


set_target_properties(
  base
  PROPERTIES
  OUTPUT_NAME imgui-base )

target_include_directories(
  base
  PUBLIC
  $<BUILD_INTERFACE:${IMGUI_SRC_ROOT}>
  $<INSTALL_INTERFACE:include/>
  )

target_link_libraries(
  base
  PUBLIC
  Threads::Threads )

install(
  TARGETS base
  ${INSTALL_TARGETS_DEFAULT_ARGS} )

install(
  FILES
  imconfig.h
  imgui.h
  imgui_internal.h
  imstb_rectpack.h
  imstb_textedit.h
  imstb_truetype.h
  DESTINATION
  include/)

add_library( imgui::base ALIAS base )

option(
  IMGUI_BUILD_DEMO
  "Build demo library"
  FALSE )

if( IMGUI_BUILD_DEMO )
  add_library(
    demo
    STATIC
    imgui_demo.cpp
    )

  set_target_properties(
    demo
    PROPERTIES
    OUTPUT_NAME imgui-demo )

  target_include_directories(
    demo
    PUBLIC
    $<BUILD_INTERFACE:${IMGUI_SRC_ROOT}>
    $<INSTALL_INTERFACE:include/>
    )

  target_link_libraries(
    demo
    PUBLIC
    base )

  install(
    TARGETS demo
    ${INSTALL_TARGETS_DEFAULT_ARGS})

  add_library( imgui::demo ALIAS demo )
endif()

add_subdirectory( backends/ )
add_subdirectory( misc )
add_subdirectory( examples )
include( CMakePackageConfigHelpers )
configure_file( imguiConfig.cmake.in imguiConfig.cmake @ONLY )

install(
  FILES "${CMAKE_CURRENT_BINARY_DIR}/imguiConfig.cmake"
  DESTINATION lib/cmake/imgui/ )

install(
  EXPORT imguiTargets
  NAMESPACE imgui::
  DESTINATION lib/cmake/imgui/ )
