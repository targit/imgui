if( IMGUI_BUILD_EXAMPLES AND IMGUI_BUILD_PLATFORM_SDL2 AND IMGUI_BUILD_RENDERER_OPENGL2 AND IMGUI_BUILD_DEMO)
  message( "Building example: sdl_opengl2" )
  add_executable(
    sdl2-opengl2
    main.cpp )
  target_link_libraries(
    sdl2-opengl2
    imgui::base
    imgui::platform-sdl2
    imgui::renderer-opengl2
    imgui::demo
    )
endif()
