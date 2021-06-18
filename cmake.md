cmake_minimum_required(VERSION 3.8)
project(top-down_video)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(OpenCV REQUIRED)
find_package(cv_bridge REQUIRED)

# camera package
add_library(
        camera
        include/camera/camera.h
        src/camera/camera.cpp
)
target_include_directories(camera PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include")
ament_target_dependencies(camera OpenCV cv_bridge)

# config file
add_library(
        config
        include/config/config.h
)
target_include_directories(camera PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include")
target_link_libraries(config camera)

# define and link executable
add_executable(video_viewer src/video_viewer.cpp)
target_link_libraries(video_viewer camera)
ament_target_dependencies(video_viewer 
  rclcpp 
  sensor_msgs 
  OpenCV 
  cv_bridge
)

install(TARGETS video_viewer
  DESTINATION lib/${PROJECT_NAME})

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()