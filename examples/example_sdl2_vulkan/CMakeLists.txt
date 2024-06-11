if( IMGUI_BUILD_EXAMPLES AND IMGUI_BUILD_PLATFORM_SDL2 AND IMGUI_BUILD_RENDERER_VULKAN AND IMGUI_BUILD_DEMO)
#  find_package( GLEW REQUIRED )
  message( "Building example: sdl_vulkan" )
  add_executable(
    sdl2-vulkan
    main.cpp )
  target_link_libraries(
    sdl2-vulkan
    imgui::base
    imgui::platform-sdl2
    imgui::renderer-vulkan
    imgui::demo
#    GLEW::GLEW
    )
endif()
