option( IMGUI_BUILD_MISC
  "Build contributed C++ utilites.  Currently only the std::string input wrapper."
  FALSE )

if( IMGUI_BUILD_MISC )
  add_library(
    imgui-misc
    imgui_stdlib.cpp )
  target_include_directories(
    imgui-misc
    PUBLIC
    $<BUILD_INTERFACE:${IMGUI_SRC_ROOT}>
    $<INSTALL_INTERFACE:include/> )
  install(
    TARGETS imgui-misc
    ${INSTALL_TARGETS_DEFAULT_ARGS})
  install(
    FILES
    imgui_stdlib.h
    DESTINATION
    include/ )
  add_library( imgui::misc ALIAS imgui-misc )    
endif()
