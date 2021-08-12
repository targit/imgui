option( IMGUI_BUILD_MISC
  "Build contributed C++ utilites.  Currently only the std::string input wrapper."
  FALSE )

if( IMGUI_BUILD_MISC )
  add_library(
    misc
    imgui_stdlib.cpp )
  target_include_directories(
    misc
    PUBLIC
    $<BUILD_INTERFACE:${IMGUI_SRC_ROOT}>
    $<INSTALL_INTERFACE:include/> )
  target_link_libraries(
    misc
    PUBLIC
    base
    )
  install(
    TARGETS misc
    ${INSTALL_TARGETS_DEFAULT_ARGS})
  install(
    FILES
    imgui_stdlib.h
    DESTINATION
    include/ )
  add_library( imgui::misc ALIAS misc )
endif()
