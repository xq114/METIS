cmake_minimum_required(VERSION 2.8)
project(METIS)

set(GKLIB_PATH "${CMAKE_SOURCE_DIR}/GKlib" CACHE PATH "path to GKlib")
set(SHARED FALSE CACHE BOOL "build a shared library")

set(METIS_INSTALL TRUE)

# Configure libmetis library.
if(SHARED)
  set(METIS_LIBRARY_TYPE SHARED)
else()
  set(METIS_LIBRARY_TYPE STATIC)
endif(SHARED)

include(${GKLIB_PATH}/GKlibSystem.cmake)
# Add include directories.
include_directories(${GKLIB_PATH})
include_directories(include)
# Recursively look for CMakeLists.txt in subdirs.
add_subdirectory("include")
add_subdirectory("libmetis")
# add_subdirectory("programs")

if(METIS_INSTALL)
  set(PRJ_NAME metis)
  set(PRJ_VER 5.1.1)
  install(EXPORT metisTargets 
    FILE ${PRJ_NAME}Targets.cmake
    DESTINATION lib/cmake/${PRJ_NAME})
  include(CMakePackageConfigHelpers)
  write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/${PRJ_NAME}ConfigVersion.cmake
    VERSION ${PRJ_VER}
    COMPATIBILITY SameMajorVersion)
  file(WRITE ${CMAKE_CURRENT_BINARY_DIR}/${PRJ_NAME}Config.cmake
    "include(\${CMAKE_CURRENT_LIST_DIR}/${PRJ_NAME}Targets.cmake)")
  install(FILES 
    ${CMAKE_CURRENT_BINARY_DIR}/${PRJ_NAME}ConfigVersion.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/${PRJ_NAME}Config.cmake
    DESTINATION lib/cmake/${PRJ_NAME})
endif()
