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
    imgui-sdl2
    imgui_impl_sdl.cpp )
  target_link_libraries(
    imgui-sdl2
    SDL2::SDL2
    imgui-base )
  target_include_directories(
    imgui-sdl2
    PUBLIC
    $<BUILD_INTERFACE:${IMGUI_SRC_ROOT}/backends/>
    $<INSTALL_INTERFACE:include/>
    )
  install(
    TARGETS imgui-sdl2
    ${INSTALL_TARGETS_DEFAULT_ARGS})
  install(
    FILES
    imgui_impl_sdl.h
    DESTINATION
    include/ )    
  add_library( imgui::platform-sdl2 ALIAS imgui-sdl2 )
endif()

if( IMGUI_BUILD_RENDERER_OPENGL2 )
  find_package( OpenGL REQUIRED )
  add_library(
    imgui-opengl2
    imgui_impl_opengl2.cpp )
  target_link_libraries(
    imgui-opengl2
    OpenGL::GL
    imgui-base )
  target_include_directories(
    imgui-opengl2
    PUBLIC
    $<BUILD_INTERFACE:${IMGUI_SRC_ROOT}/backends/>
    $<INSTALL_INTERFACE:include/>
    )
  install(
    TARGETS imgui-opengl2
    ${INSTALL_TARGETS_DEFAULT_ARGS})
  install(
    FILES
    imgui_impl_opengl2.h
    DESTINATION
    include/ )    
  add_library( imgui::renderer-opengl2 ALIAS imgui-opengl2 )
endif()

if( IMGUI_BUILD_RENDERER_OPENGL3 )
  find_package( OpenGL REQUIRED )
  add_library(
    imgui-opengl3
    imgui_impl_opengl3.cpp )
  target_link_libraries(
    imgui-opengl3
    OpenGL::GL
    imgui-base )
  target_include_directories(
    imgui-opengl3
    PUBLIC
    $<BUILD_INTERFACE:${IMGUI_SRC_ROOT}/backends/>
    $<INSTALL_INTERFACE:include/>
    )
  install(
    TARGETS imgui-opengl3
    ${INSTALL_TARGETS_DEFAULT_ARGS})
  install(
    FILES
    imgui_impl_opengl3.h
    DESTINATION
    include/ )    
  add_library( imgui::renderer-opengl3 ALIAS imgui-opengl3 )
endif()

if( IMGUI_BUILD_RENDERER_VULKAN )
  find_package( Vulkan REQUIRED )
  add_library(
    imgui-vulkan
    imgui_impl_vulkan.cpp )
  target_link_libraries(
    imgui-vulkan
    Vulkan::Vulkan
    imgui-base )
  target_include_directories(
    imgui-vulkan
    PUBLIC
    $<BUILD_INTERFACE:${IMGUI_SRC_ROOT}/backends/>
    $<INSTALL_INTERFACE:include/>
    )
  install(
    TARGETS imgui-vulkan
    ${INSTALL_TARGETS_DEFAULT_ARGS})
  install(
    FILES
    imgui_impl_vulkan.h
    DESTINATION
    include/ )    
  add_library( imgui::renderer-vulkan ALIAS imgui-vulkan )
endif()
