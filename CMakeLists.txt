cmake_minimum_required(VERSION 3.8)
project(pc_utm_to_mgrs_converter)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# Use C++17
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Find dependencies
find_package(ament_cmake_auto REQUIRED)
ament_auto_find_build_dependencies()

find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(PCL REQUIRED COMPONENTS io)
find_package(pcl_conversions REQUIRED)

# GeographicLib
find_package(GeographicLib QUIET)
if(NOT GeographicLib_FOUND)
  include(CMakeFindDependencyMacro)
  find_dependency(GeographicLib)
endif()

if(NOT GeographicLib_FOUND)
  message(FATAL_ERROR "GeographicLib not found. Please install GeographicLib or set GeographicLib_DIR.")
endif()

# Create executable
add_executable(${PROJECT_NAME} src/pc_utm_to_mgrs_converter.cpp)

# Specify include directories
target_include_directories(${PROJECT_NAME}
  PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
  PRIVATE
    ${PCL_INCLUDE_DIRS}
    ${GeographicLib_INCLUDE_DIRS}
)

# Link libraries
target_link_libraries(${PROJECT_NAME}
  ${PCL_LIBRARIES}
  ${GeographicLib_LIBRARIES}
)

# Specify dependencies
ament_target_dependencies(${PROJECT_NAME}
  rclcpp
  std_msgs
  PCL
  pcl_conversions
)

# Install targets
install(TARGETS ${PROJECT_NAME}
  DESTINATION lib/${PROJECT_NAME}
)

# Install launch and config files if they exist
install(DIRECTORY
  launch
  config
  DESTINATION share/${PROJECT_NAME}
  OPTIONAL
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  set(ament_cmake_copyright_FOUND TRUE)
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_auto_package()
