if( IMGUI_BUILD_EXAMPLES AND IMGUI_BUILD_PLATFORM_SDL2 AND IMGUI_BUILD_RENDERER_OPENGL3 AND IMGUI_BUILD_DEMO)
  find_package( GLEW REQUIRED )
  message( "Building example: sdl_opengl3" )
  add_executable(
    sdl2-opengl3
    main.cpp )
  target_link_libraries(
    sdl2-opengl3
    imgui::base
    imgui::platform-sdl2
    imgui::renderer-opengl3
    imgui::demo
    GLEW::GLEW
    )
endif()
