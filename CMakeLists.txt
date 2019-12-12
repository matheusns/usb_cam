cmake_minimum_required(VERSION 2.8.3)
project(usb_cam)

## Compile as C++11, supported in ROS Kinetic and newer
add_compile_options(-std=c++11)

set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} "-std=c++11 -pthread -w")

find_package(catkin REQUIRED COMPONENTS
  cv_bridge
  image_transport
  roscpp
  sensor_msgs
  std_msgs
  roslint
)

find_package(OpenCV 3 REQUIRED)

set(CPP_SRC 
  src/usb_cam_pub.cpp
  )

set(ROSLINT_CPP_OPTS "--extensions=hpp,cpp,c,hh,cc,h")

roslint_cpp(
  ${CPP_SRC}
  )

catkin_package(
    INCLUDE_DIRS
    DEPENDS        OpenCV
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
)

# Declare a C++ library
add_library(${PROJECT_NAME} ${CPP_SRC})

add_executable(${PROJECT_NAME}_node
  ${CPP_SRC})

target_link_libraries(${PROJECT_NAME}_node
  ${catkin_LIBRARIES}
  ${OpenCV_LIBRARIES}
)

if (CATKIN_ENABLE_TESTING)
  find_package(cmake_modules)
  find_package(rostest REQUIRED)
  find_package(OpenCV REQUIRED)

  include_directories(
    ${PROJECT_SOURCE_DIR}/include
    ${catkin_INCLUDE_DIRS}
    ${OpenCV_INCLUDE_DIRS})

  roslint_add_test()
endif()
