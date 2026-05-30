# module files
install(DIRECTORY lib/mod
        DESTINATION ${CMAKE_INSTALL_LIBDIR}
        COMPONENT pymod_run
        FILES_MATCHING PATTERN "*.py" PATTERN "*.pyi")

# redirector
# -------------------------------------------------------------------------

set(exportDir ${CMAKE_CURRENT_BINARY_DIR}/redirector)
file(MAKE_DIRECTORY ${exportDir}/mod)
# TODO: should be file(COPY_FILE redirector/pyproject.toml ${exportDir}/pyproject.toml) when CMake 3.21 is required
configure_file(redirector/pyproject.toml ${exportDir}/pyproject.toml @ONLY)
configure_file(redirector/setup.cfg.in ${exportDir}/setup.cfg @ONLY)
configure_file(redirector/mod/__init__.py.in ${exportDir}/mod/__init__.py @ONLY)
if(${BUILD_PY_MOD_PIP})
    install(CODE "execute_process(COMMAND ${Python3_EXECUTABLE} -m pip install . WORKING_DIRECTORY ${exportDir})"
            COMPONENT pymod_run)
endif()

# mod
# -------------------------------------------------------------------------
set(exportDir ${CMAKE_CURRENT_BINARY_DIR}/bin)
configure_file(bin/mod.in ${exportDir}/mod @ONLY)
install(PROGRAMS ${exportDir}/mod
        DESTINATION ${CMAKE_INSTALL_BINDIR}
        COMPONENT pymod_run)
