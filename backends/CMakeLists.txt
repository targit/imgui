option( IMGUI_BUILD_PLATFORM_SDL2
  "Build the SDL2 platform backend.  Note, you will still need a rendering backend."
  FALSE )
# option( IMGUI_BUILD_PLATFORM_GLUT
#   "Build the Glut platform backend. Note, you will still need a rendering backend."
#   FALSE )
option( IMGUI_BUILD_RENDERER_OPENGL2
  "Build the OpenGL 2 rendering backend. Note, you will still need a platform backend."
  FALSE )
option( IMGUI_BUILD_RENDERER_OPENGL3
  "Build the OpenGL 3 rendering backend. Note, you will still need a platform backend."
  FALSE )
option( IMGUI_BUILD_RENDERER_VULKAN
  "Build the Vulkan rendering backend. Note, you will still need a platform backend."
  FALSE )

if( IMGUI_BUILD_PLATFORM_SDL2 )
  find_package( SDL2 REQUIRED )
  add_library(
    platform-sdl2
    imgui_impl_sdl.cpp )
  target_link_libraries(
    platform-sdl2
    SDL2::SDL2
    imgui::base )
  target_include_directories(
    platform-sdl2
    PUBLIC
    $<BUILD_INTERFACE:${IMGUI_SRC_ROOT}/backends/>
    $<INSTALL_INTERFACE:include/>
    )
  install(
    TARGETS platform-sdl2
    ${INSTALL_TARGETS_DEFAULT_ARGS})
  install(
    FILES
    imgui_impl_sdl.h
    DESTINATION
    include/ )    
  add_library( imgui::platform-sdl2 ALIAS platform-sdl2 )
endif()

if( IMGUI_BUILD_RENDERER_OPENGL2 )
  find_package( OpenGL REQUIRED )
  add_library(
    renderer-opengl2
    imgui_impl_opengl2.cpp )
  target_link_libraries(
    renderer-opengl2
    OpenGL::GL
    imgui::base )
  target_include_directories(
    renderer-opengl2
    PUBLIC
    $<BUILD_INTERFACE:${IMGUI_SRC_ROOT}/backends/>
    $<INSTALL_INTERFACE:include/>
    )
  install(
    TARGETS renderer-opengl2
    ${INSTALL_TARGETS_DEFAULT_ARGS})
  install(
    FILES
    imgui_impl_opengl2.h
    DESTINATION
    include/ )    
  add_library( imgui::renderer-opengl2 ALIAS renderer-opengl2 )
endif()

if( IMGUI_BUILD_RENDERER_OPENGL3 )
  find_package( OpenGL REQUIRED )
  add_library(
    renderer-opengl3
    imgui_impl_opengl3.cpp )
  target_link_libraries(
    renderer-opengl3
    OpenGL::GL
    imgui::base )
  target_include_directories(
    renderer-opengl3
    PUBLIC
    $<BUILD_INTERFACE:${IMGUI_SRC_ROOT}/backends/>
    $<INSTALL_INTERFACE:include/>
    )
  install(
    TARGETS renderer-opengl3
    ${INSTALL_TARGETS_DEFAULT_ARGS})
  install(
    FILES
    imgui_impl_opengl3.h
    DESTINATION
    include/ )    
  add_library( imgui::renderer-opengl3 ALIAS renderer-opengl3 )
endif()

if( IMGUI_BUILD_RENDERER_VULKAN )
  find_package( Vulkan REQUIRED )
  add_library(
    renderer-vulkan
    imgui_impl_vulkan.cpp )
  target_link_libraries(
    renderer-vulkan
    Vulkan::Vulkan
    imgui::base )
  target_include_directories(
    renderer-vulkan
    PUBLIC
    $<BUILD_INTERFACE:${IMGUI_SRC_ROOT}/backends/>
    $<INSTALL_INTERFACE:include/>
    )
  install(
    TARGETS renderer-vulkan
    ${INSTALL_TARGETS_DEFAULT_ARGS})
  install(
    FILES
    imgui_impl_vulkan.h
    DESTINATION
    include/ )    
  add_library( imgui::renderer-vulkan ALIAS renderer-vulkan )
endif()
