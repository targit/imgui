# Oh well, this looks like a license clusterfuck to me.
# Better make this optional.

option( IMGUI_INSTALL_FONTS
  "Whether to install the fonts shadily distributed along imgui."
  FALSE )

install(
  FILES
  Cousine-Regular.ttf
  DroidSans.ttf
  Karla-Regular.ttf
  ProggyClean.ttf
  ProggyTiny.ttf
  Roboto-Medium.ttf
  DESTINATION
  share/imgui/
  )
