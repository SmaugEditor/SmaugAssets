# Installs the assets 

include(GNUInstallDirs)

file( GLOB ASSETS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
	assets/*)

list(APPEND ASSETS
	imgui.ini
	LICENSE)

# Phony target to invoke copies
add_custom_target(assets-target)

# Copy to build directory post build
foreach(F ${ASSETS})
	add_custom_command(
		TARGET assets-target POST_BUILD
		COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/${F} ${CMAKE_BINARY_DIR}/${F}
		MAIN_DEPENDENCY ${CMAKE_CURRENT_SOURCE_DIR}/${F})
endforeach()
add_dependencies(smaug assets-target)

# Install rule for the assets. Going to put this in $PREFIX/share/smaug 
install(FILES ${ASSETS} DESTINATION ${CMAKE_INSTALL_DATADIR}/smaug/)
