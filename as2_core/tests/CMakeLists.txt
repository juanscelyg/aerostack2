find_package(ament_cmake_gtest REQUIRED)
find_package(Eigen3 REQUIRED)

file(GLOB TEST_SOURCES *.cpp)

# create a test executable for each test file
foreach(TEST_SOURCE ${TEST_SOURCES})

  get_filename_component(_src_filename ${TEST_SOURCE} NAME)
  string(LENGTH ${_src_filename} name_length)
  math(EXPR final_length "${name_length}-4") # remove .cpp of the name
  string(SUBSTRING ${_src_filename} 0 ${final_length} TEST_NAME)

  ament_add_gtest(${TEST_NAME} ${TEST_SOURCE})
  target_include_directories(${TEST_NAME} PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/../include
    ${EIGEN3_INCLUDE_DIRS}
  )

  target_link_libraries(${TEST_NAME}
    ${as2_msgs_TARGETS}
    ${geographic_msgs_TARGETS}
    ${geometry_msgs_TARGETS}
    ${nav_msgs_TARGETS}
    ${sensor_msgs_TARGETS}
    ${std_msgs_TARGETS}
    ${std_srvs_TARGETS}
    cv_bridge::cv_bridge
    geographic_msgs::validation
    image_transport::image_transport
    rclcpp::rclcpp
    rclcpp_action::rclcpp_action
    rclcpp_lifecycle::rclcpp_lifecycle
    sensor_msgs::sensor_msgs_library
    tf2::tf2
    tf2_geometry_msgs::tf2_geometry_msgs
    tf2_ros::static_transform_broadcaster_node
    tf2_ros::tf2_ros
    ${PROJECT_NAME}
    yaml-cpp::yaml-cpp
  )
endforeach()
