cmake_minimum_required(VERSION 3.15)
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_LIST_DIR}/cmake)

###########################################################################
# Package Properties
###########################################################################
file(READ "VERSION" PROJECT_VERSION)
string(STRIP "${PROJECT_VERSION}" PROJECT_VERSION) # remove the newline

set(mod_VERSION ${PROJECT_VERSION} CACHE INTERNAL "" FORCE)
set(PNAME_FILE mod) # how the project name should be written in file names
set(mod_AUTHOR "Jakob Lykke Andersen")
set(mod_AUTHOR_EMAIL "jlandersen@imada.sdu.dk")
set(CPACK_PACKAGE_CONTACT "${mod_AUTHOR} <${mod_AUTHOR_EMAIL}>")


###########################################################################
# Options
###########################################################################
option(BUILD_PY_MOD "Enable building of the Python bindings." ON)
option(BUILD_PY_MOD_PIP "If BUILD_PY_MOD, then install the bindings also with pip in the default location." ON)

option(BUILD_TESTING "Enable test building." ON)

set(languages CXX)
project(${PNAME_FILE} VERSION ${PROJECT_VERSION} LANGUAGES ${languages})

enable_testing()  # should be included here to add the targets in the top-level folder

###########################################################################
# Individual checks
###########################################################################
if(NOT CMAKE_CXX_STANDARD)
    set(CMAKE_CXX_STANDARD 17)
    set(CMAKE_CXX_STANDARD_REQUIRED ON)
endif()

set(libmod_config_dependencies "")
set(libmod_config_find_files "")


find_package(LATEX REQUIRED COMPONENTS PDFLATEX)

# Python
# -------------------------------------------------------------------------
if(BUILD_PY_MOD)
    find_package(Python3 REQUIRED COMPONENTS Interpreter Development)
    string(APPEND libmod_config_dependencies "find_dependency(Python3 REQUIRED COMPONENTS Interpreter Development)\n")
endif()


###########################################################################
# Targets and Artefacts
###########################################################################
include(GNUInstallDirs)

if(BUILD_PY_MOD)
    add_subdirectory(libs/pymod)
endif()
if(BUILD_TESTING)
    add_subdirectory(test)
endif()
