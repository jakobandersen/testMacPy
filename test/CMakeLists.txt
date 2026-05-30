if(NOT TARGET tests)
    add_custom_target(tests)
endif()
set(workDir ${CMAKE_CURRENT_BINARY_DIR}/workDir/pymod-module-direct)
file(MAKE_DIRECTORY ${workDir})
add_test(NAME pymod-module-direct
		COMMAND ${CMAKE_INSTALL_FULL_BINDIR}/mod --nopost -e "assert not mod._redirected"
		WORKING_DIRECTORY ${workDir})
set(workDir ${CMAKE_CURRENT_BINARY_DIR}/workDir/pymod-module-redirected)
file(MAKE_DIRECTORY ${workDir})
add_test(NAME pymod-module-redirected
		COMMAND ${Python3_EXECUTABLE} -c "import mod\nassert mod._redirected"
		WORKING_DIRECTORY ${workDir})
