cmake_minimum_required(VERSION 2.8.12)
project(ur_ik_plugin)

add_compile_options(-std=c++11 -O3)

find_package(catkin REQUIRED COMPONENTS
  moveit_core
  pluginlib
  roscpp
  tf_conversions
)

include_directories(${catkin_INCLUDE_DIRS})

catkin_package(
  LIBRARIES
  CATKIN_DEPENDS
    moveit_core
    pluginlib
    roscpp
    tf_conversions
)

include_directories(include)

# set(IKFAST_LIBRARY_NAME ur5_manipulator_moveit_ikfast_plugin)

find_package(LAPACK REQUIRED)

add_library(ur5_ik_moveit_plugin src/ur_ik_moveit_plugin.cpp)
set_target_properties(ur5_ik_moveit_plugin PROPERTIES COMPILE_DEFINITIONS "UR5_PARAMS")
target_link_libraries(ur5_ik_moveit_plugin ${catkin_LIBRARIES} ${Boost_LIBRARIES} ${LAPACK_LIBRARIES})

install(TARGETS ur5_ik_moveit_plugin LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION})

install(
  FILES
  ur_ik_plugin.xml
  DESTINATION
  ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
