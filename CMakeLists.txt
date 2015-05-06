cmake_minimum_required(VERSION 2.8.3)

project(axis_rqt_plugin)
# Load catkin and all dependencies required for this package
find_package(catkin REQUIRED COMPONENTS rqt_gui rqt_gui_cpp image_transport sensor_msgs cv_bridge rosconsole)

find_package(Qt4 COMPONENTS QtCore QtGui REQUIRED)

include(${QT_USE_FILE})


set(rqt_axis_teleop_SRCS
  src/rqt_axis_teleop/axis_teleop.cpp
  src/rqt_axis_teleop/ratio_layouted_frame.cpp
)

set(rqt_axis_teleop_HDRS
  include/rqt_axis_teleop/axis_teleop.h
  include/rqt_axis_teleop/ratio_layouted_frame.h
)

set(rqt_axis_teleop_UIS
  src/rqt_axis_teleop/axis_teleop.ui
)


set(axis_rqt_plugin_INCLUDE_DIRECTORIES
  include
  ${CMAKE_CURRENT_BINARY_DIR}
)

catkin_package(
	INCLUDE_DIRS include
	LIBRARIES ${PROJECT_NAME} 
	CATKIN_DEPENDS rqt_gui rqt_gui_cpp image_transport sensor_msgs cv_bridge rosconsole
)
catkin_python_setup()


qt4_wrap_cpp(rqt_axis_teleop_MOCS ${rqt_axis_teleop_HDRS})
qt4_wrap_ui(rqt_axis_teleop_UIS_H ${rqt_axis_teleop_UIS})


include_directories(${axis_rqt_plugin_INCLUDE_DIRECTORIES} ${catkin_INCLUDE_DIRS})
add_library(${PROJECT_NAME}
                ${rqt_axis_teleop_SRCS} ${rqt_axis_teleop_MOCS} ${rqt_axis_teleop_UIS_H}
)
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES} ${QT_QTCORE_LIBRARY} ${QT_QTGUI_LIBRARY})

find_package(class_loader)
class_loader_hide_library_symbols(${PROJECT_NAME})

install(FILES plugin.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION})

install(DIRECTORY include/
  DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
)
