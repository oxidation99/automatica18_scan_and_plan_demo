cmake_minimum_required(VERSION 2.8.3)
project(snp_demo_gui)

add_definitions(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  actionlib
  class_loader
  godel_msgs
  moveit_ros_planning_interface
  pluginlib
  roscpp
  rviz
)

find_package(Qt5Widgets REQUIRED)
find_package(Qt5Core REQUIRED)
find_package(Qt5Gui REQUIRED)

catkin_package()

set(${PROJECT_NAME}_SRCS
  src/command_widget.cpp
  src/demo_panel.cpp
  src/demo_widget.cpp
  src/godel_bridge.cpp
)

set(${PROJECT_NAME}_HDRS
  src/command_widget.h
  src/demo_panel.h
  src/demo_widget.h
)

set(${PROJECT_NAME}_UIS
  src/ui/command_widget.ui
  src/ui/demo_widget.ui
)

add_definitions(-DQT_NO_KEYWORDS)
qt5_wrap_cpp(${PROJECT_NAME}_MOCS ${${PROJECT_NAME}_HDRS})
qt5_wrap_ui(${PROJECT_NAME}_UIS_H ${${PROJECT_NAME}_UIS})

include_directories(${CMAKE_CURRENT_BINARY_DIR} ${catkin_INCLUDE_DIRS})

## Declare a cpp library
add_library(${PROJECT_NAME}
  ${${PROJECT_NAME}_UIS_H}
  ${${PROJECT_NAME}_MOCS}
  ${${PROJECT_NAME}_SRCS}
)

target_link_libraries(${PROJECT_NAME}
                      ${catkin_LIBRARIES}
                      Qt5::Widgets
                      Qt5::Gui
                      )

add_dependencies(${PROJECT_NAME} ${catkin_EXPORTED_TARGETS})

class_loader_hide_library_symbols(${PROJECT_NAME})

# Install
install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION})

install(FILES plugin.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
