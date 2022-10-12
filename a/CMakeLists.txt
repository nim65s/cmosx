cmake_minimum_required(VERSION 3.15)
project(MathFunctions)

include(GNUInstallDirs)

add_library(MathFunctions SHARED MathFunctions.cxx MathFunctions.h)

target_include_directories(
  MathFunctions PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>"
                       "$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>")

install(
  TARGETS MathFunctions
  EXPORT MathFunctionsTargets
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
  INCLUDES
  DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

install(FILES MathFunctions.h DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

install(
  EXPORT MathFunctionsTargets
  FILE MathFunctionsTargets.cmake
  NAMESPACE MathFunctions::
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/MathFunctions)
include(CMakePackageConfigHelpers)

configure_package_config_file(
  ${CMAKE_CURRENT_SOURCE_DIR}/Config.cmake.in
  "${CMAKE_CURRENT_BINARY_DIR}/MathFunctionsConfig.cmake"
  INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/MathFunctions)
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/MathFunctionsConfig.cmake"
              "${CMAKE_CURRENT_BINARY_DIR}/MathFunctionsConfigVersion.cmake"
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/MathFunctions)

set(version 3.4.1)

set_property(TARGET MathFunctions PROPERTY VERSION ${version})
set_property(TARGET MathFunctions PROPERTY SOVERSION 3)
set_property(TARGET MathFunctions PROPERTY INTERFACE_MathFunctions_MAJOR_VERSION
                                           3)
set_property(
  TARGET MathFunctions
  APPEND
  PROPERTY COMPATIBLE_INTERFACE_STRING MathFunctions_MAJOR_VERSION)

# generate the version file for the config file
write_basic_package_version_file(
  "${CMAKE_CURRENT_BINARY_DIR}/MathFunctionsConfigVersion.cmake"
  VERSION "${version}"
  COMPATIBILITY AnyNewerVersion)

include(CTest)
add_executable(test-sqrt test-sqrt.cpp)
target_link_libraries(test-sqrt PRIVATE MathFunctions)
add_test(NAME test-sqrt COMMAND test-sqrt)
