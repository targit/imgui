cmake_minimum_required( VERSION 3.21 )
cmake_policy( SET CMP0048 NEW )
project( imgui VERSION 1.90.8 LANGUAGES CXX)


set( IMGUI_SRC_ROOT ${CMAKE_CURRENT_SOURCE_DIR})
set( IMGUI_BUILD_ROOT ${CMAKE_CURRENT_BINARY_DIR})

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

if( IMGUI_CONFIG_FREETYPE )
  find_package( Freetype  REQUIRED )
  set( IMGUI_BASE_FREETYPE_LIBS Freetype::Freetype )
  set( IMGUI_BASE_FREETYPE_INCS $<BUILD_INTERFACE:${IMGUI_SRC_ROOT}/misc/freetype/> )
  set( IMGUI_BASE_FREETYPE_CODE ${IMGUI_SRC_ROOT}/misc/freetype/imgui_freetype.cpp )
else()
  set( IMGUI_BASE_FREETYPE_LIBS  )
endif()


add_library(
  base
  STATIC
  imgui.cpp
  imgui_draw.cpp
  imgui_tables.cpp
  imgui_widgets.cpp
  ${IMGUI_BASE_FREETYPE_CODE}
  )


set_target_properties(
  base
  PROPERTIES
  OUTPUT_NAME imgui-base )

target_include_directories(
  base
  PUBLIC
  $<BUILD_INTERFACE:${IMGUI_SRC_ROOT}>
  $<BUILD_INTERFACE:${IMGUI_BUILD_ROOT}>
  ${IMGUI_BASE_FREETYPE_INCS}
  $<INSTALL_INTERFACE:include/>
  )

target_link_libraries(
  base
  PUBLIC
  Threads::Threads
  ${IMGUI_BASE_FREETYPE_LIBS})

install(
  TARGETS base
  ${INSTALL_TARGETS_DEFAULT_ARGS} )

install(
  FILES
  ${IMGUI_BUILD_ROOT}/imconfig.h
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

option(
  IMGUI_CONFIG_32BIT_INDICES
  "Use 32bit vertex indices"
  FALSE )

option(
  IMGUI_CONFIG_FREETYPE
  "Use freetype for fonts. (Requires libfreetype)"
  FALSE )

option(
  IMGUI_CONFIG_DISABLE_OBSOLETE_FUNCTIONS
  "Disable obsolete functions"
  TRUE )

if( IMGUI_CONFIG_32BIT_INDICES )
  set(IMGUI_CONF_32BIT_FLAG 1 )
else()
  set(IMGUI_CONF_32BIT_FLAG 0 )
endif()

if( IMGUI_CONFIG_FREETYPE )
  set( IMGUI_CONF_FREETYPE_FLAG 1 )
else()
  set( IMGUI_CONF_FREETYPE_FLAG 0 )
endif()

if( IMGUI_CONFIG_DISABLE_OBSOLETE_FUNCTIONS )
  set( IMGUI_CONF_DISABLE_OBSOLETE_FUNCTIONS 1 )
else()
  set( IMGUI_CONF_DISABLE_OBSOLETE_FUNCTIONS 0 )
endif()

configure_file( imconfig.h.in imconfig.h @ONLY )


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
configure_package_config_file(
  imguiConfig.cmake.in
  imguiConfig.cmake
  INSTALL_DESTINATION lib/cmake/imgui )
# @ONLY )
write_basic_package_version_file(
  imguiConfigVersion.cmake
  VERSION ${CMAKE_PROJECT_VERSION}
  COMPATIBILITY ExactVersion )

configure_file( imgui-opengl2.pc.in imgui-opengl2.pc @ONLY )
configure_file( imgui-opengl3.pc.in imgui-opengl3.pc @ONLY )
configure_file( imgui-vulkan.pc.in imgui-vulkan.pc @ONLY )


install(
  FILES
  "${CMAKE_CURRENT_BINARY_DIR}/imguiConfig.cmake"
  "${CMAKE_CURRENT_BINARY_DIR}/imguiConfigVersion.cmake"
  DESTINATION lib/cmake/imgui/ )


install(
  EXPORT imguiTargets
  NAMESPACE imgui::
  DESTINATION lib/cmake/imgui/ )

configure_file( imgui.pc.in imgui.pc @ONLY )
install(
  FILES "${CMAKE_CURRENT_BINARY_DIR}/imgui.pc"
  DESTINATION lib/pkgconfig/ )

if( IMGUI_BUILD_DEMO )
  configure_file( imgui-demo.pc.in imgui-demo.pc @ONLY )
    install(
    FILES "${CMAKE_CURRENT_BINARY_DIR}/imgui-demo.pc"
    DESTINATION lib/pkgconfig/ )
endif()

if( IMGUI_BUILD_PLATFORM_SDL2 )
  configure_file( imgui-sdl2.pc.in imgui-sdl2.pc @ONLY )
  install(
    FILES "${CMAKE_CURRENT_BINARY_DIR}/imgui-sdl2.pc"
    DESTINATION lib/pkgconfig/ )
endif()

if( IMGUI_BUILD_RENDERER_OPENGL2 )
  configure_file( imgui-opengl2.pc.in imgui-opengl2.pc @ONLY )
  install(
    FILES "${CMAKE_CURRENT_BINARY_DIR}/imgui-opengl2.pc"
    DESTINATION lib/pkgconfig/ )
endif()

if( IMGUI_BUILD_RENDERER_OPENGL3 )
  configure_file( imgui-opengl3.pc.in imgui-opengl3.pc @ONLY )
  install(
    FILES "${CMAKE_CURRENT_BINARY_DIR}/imgui-opengl3.pc"
    DESTINATION lib/pkgconfig/ )
endif()

if( IMGUI_BUILD_RENDERER_VULKAN )
  configure_file( imgui-vulkan.pc.in imgui-vulkan.pc @ONLY )
  install(
    FILES "${CMAKE_CURRENT_BINARY_DIR}/imgui-vulkan.pc"
    DESTINATION lib/pkgconfig/ )
endif()
