cmake_minimum_required(VERSION 2.8.3)
project(eufsa_msgs)

find_package(catkin REQUIRED COMPONENTS
  genmsg
  std_msgs
  sensor_msgs
  geometry_msgs
  diagnostic_msgs
)

## System dependencies are found with CMake's conventions
find_package(Boost REQUIRED COMPONENTS system)
find_package(Boost REQUIRED thread)


add_message_files(
  DIRECTORY msg
  FILES
  wheelSpeeds.msg
  canState.msg
)

generate_messages(DEPENDENCIES
  std_msgs
  sensor_msgs
  geometry_msgs
  diagnostic_msgs
)

catkin_package()