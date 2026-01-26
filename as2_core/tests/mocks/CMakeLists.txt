# File to include, use parent CMakelists path

# Add mocks library
set(MOCK_SOURCES
  tests/mocks/aerial_platform/mock_aerial_platform.cpp
  tests/mocks/executor_thread_util/executor_thread_util.cpp
)

find_package(Eigen3 REQUIRED)

add_library(${MOCKS_LIBRARY_NAME} SHARED ${MOCK_SOURCES})
target_include_directories(${MOCKS_LIBRARY_NAME} PRIVATE
  ${CMAKE_CURRENT_SOURCE_DIR}/include
  ${EIGEN3_INCLUDE_DIRS}
)
target_link_libraries(mocks PUBLIC
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
  )

install(
  DIRECTORY tests/mocks/
  DESTINATION include/${PROJECT_NAME}/${MOCKS_LIBRARY_NAME}/
  FILES_MATCHING PATTERN "*.hpp"
)