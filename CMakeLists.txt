
########################################################
# If your cmake is older than 3.4 - goodbye.
cmake_minimum_required(VERSION 3.10.0 FATAL_ERROR)

########################################################################
# Ready to start the "CMake Modules"
project(CMLibs-CMake-Modules VERSION 1.0.0 LANGUAGES CXX)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/Scripts)
include(CMLMiscFunctions)
include(CMLMultiConfigEnvironment)
tidy_gui_variables()
if (CMLIBS_HAVE_MULTICONFIG_ENV)
    mark_as_advanced(CMAKE_CONFIGURATION_TYPES)
else ()
    mark_as_advanced(CMAKE_BUILD_TYPE)
endif ()

if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release CACHE PATH "Choose the type of build, options are: None(CMAKE_CXX_FLAGS or CMAKE_C_FLAGS used) Debug Release RelWithDebInfo MinSizeRel." FORCE)
endif ()

# Standard installation scenario and the module path can be found relative to this directory
get_filename_component(_INSTALL_PREFIX "${CMAKE_CURRENT_LIST_DIR}/../../install/" REALPATH)

if (GENERATE_COMPONENTS)
    string(REPLACE ";" "-semi-colon-" GENERATE_COMPONENTS_LIST "${GENERATE_COMPONENTS}")
    set(GENERATE_COMPONENTS_OPTION -DGENERATE_COMPONENTS=${GENERATE_COMPONENTS_LIST})
    unset(GENERATE_COMPONENTS CACHE)
endif ()
add_custom_target(generate_find_module_wrappers
    COMMAND ${CMAKE_COMMAND} ${GENERATE_COMPONENTS_OPTION} -DFINDXXX_TEMPLATE=${CMAKE_CURRENT_SOURCE_DIR}/Templates/FindXXX.template.cmake -P ${CMAKE_CURRENT_SOURCE_DIR}/Scripts/CMLGenerateFindModuleWrappers.cmake
)

set(CMAKE_MODULES_INSTALL_PREFIX "${_INSTALL_PREFIX}" CACHE PATH "Install path prefix, prepended onto install directories.")
if (DEFINED INSTALL_PREFIX)
    set(CMAKE_MODULES_INSTALL_PREFIX "${INSTALL_PREFIX}" CACHE PATH "Install path prefix, prepended onto install directories." FORCE)
endif ()
set(CMAKE_INSTALL_PREFIX "${CMAKE_MODULES_INSTALL_PREFIX}" CACHE INTERNAL "Internalise install path prefix." FORCE)

set(FIND_MPI_MODULE_FILE
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindMPI.cmake
)
set(MODULES_FILES_LIST
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FileSyncedOptions.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindFTGL.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindGLEW.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindGTKGLext.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindGit.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindImageMagick.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindJPEG.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindLibGDCM.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindLibraryUtilityFunctions.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindNETGEN.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindOpt++.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindOSMesa.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindPNG.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindPackageMultipass.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindPython.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindPython2.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindPython3.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindTIFF.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindwxWidgets.cmake
)
set(FINDPYTHON_MODULES_FILES_LIST
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindPython/Support.cmake
)
set(FIND_MODULE_WRAPPERS_FILES_LIST
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindModuleWrappers/FindBZip2.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindModuleWrappers/FindClang.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindModuleWrappers/FindFTGL.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindModuleWrappers/FindFreetype.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindModuleWrappers/FindGDCM-ABI.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindModuleWrappers/FindGLEW.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindModuleWrappers/FindGTest.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindModuleWrappers/FindHDF5.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindModuleWrappers/FindITK.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindModuleWrappers/FindImageMagick.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindModuleWrappers/FindJPEG.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindModuleWrappers/FindLLVM.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindModuleWrappers/FindLibXml2.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindModuleWrappers/FindNETGEN.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindModuleWrappers/FindOPTPP.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindModuleWrappers/FindPNG.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindModuleWrappers/FindSZIP.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindModuleWrappers/FindTIFF.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Modules/FindModuleWrappers/FindZLIB.cmake
)
set(CMLIBS_MODULES_FILES_LIST
    ${CMAKE_CURRENT_SOURCE_DIR}/Scripts/CMLMiscFunctions.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Scripts/CMLCheckSDKInstallation.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/Scripts/CMLMultiConfigEnvironment.cmake
)

install(FILES ${MODULES_FILES_LIST}
    DESTINATION share/cmake/Modules
    COMPONENT CMakeFiles
)
install(FILES ${FIND_MODULE_WRAPPERS_FILES_LIST}
    DESTINATION share/cmake/Modules/FindModuleWrappers
    COMPONENT CMakeFiles
)
install(FILES ${CMLIBS_MODULES_FILES_LIST}
    DESTINATION share/cmake/Modules/CMLIBS
    COMPONENT CMakeFiles
)
install(FILES ${FINDPYTHON_MODULES_FILES_LIST}
    DESTINATION share/cmake/Modules/FindPython
    COMPONENT SDK
)

set(PRINT_SUMMARY TRUE)
if (PRINT_SUMMARY)
    message(STATUS "@")
    message(STATUS "@@@@@@@@@@@@@@@@@@@@@@ SUMMARY @@@@@@@@@@@@@@@@@@@@@@@")
    message(STATUS "@")
    message(STATUS "@ CMLIBS_CMAKE_MODULE_PATH: ${CMAKE_INSTALL_PREFIX}/share/cmake/Modules")
    message(STATUS "@")
    message(STATUS "@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@")
    message(STATUS "@")
endif ()
