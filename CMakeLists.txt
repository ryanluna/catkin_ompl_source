# This was cobbled together by Ryan Luna to provide a current Catkin build of OMPL on ROS Hydro

cmake_minimum_required(VERSION 2.8)
if(NOT ${CMAKE_VERSION} VERSION_LESS 3.0.0)
    cmake_policy(SET CMP0042 OLD)
endif()
project(ompl_source)

if(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/CMakeModules")
include(GNUInstallDirs)
include(CompilerSettings)
include(OMPLVersion)
include(OMPLUtils)

set(OMPL_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/src")
include_directories("${OMPL_INCLUDE_DIR}")

find_package(Boost COMPONENTS date_time thread serialization filesystem system program_options unit_test_framework REQUIRED)
include_directories(SYSTEM ${Boost_INCLUDE_DIR})

if(${Boost_VERSION} LESS 105300)
  # Include bundled version of boost::odeint if it isn't installed natively
  set(ODEINT_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/src/external")
  include_directories(SYSTEM "${ODEINT_INCLUDE_DIR}")
endif()

# Barebones compilation only
set(OMPL_EXTENSION_MORSE 0)
set(OMPL_EXTENSION_OPENDE 0)
set(OMPL_EXTENSION_TRIANGLE 0)

# CATKIN stuff
catkin_package(
  LIBRARIES
    ompl_source
  INCLUDE_DIRS
    src
)

add_subdirectory(src)
