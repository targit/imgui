option( IMGUI_BUILD_EXAMPLES
  "Build all of the examples for which the corresponding backends are selected."
  FALSE )

add_subdirectory( example_sdl_opengl2 )
add_subdirectory( example_sdl_opengl3 )
add_subdirectory( example_sdl_vulkan )
