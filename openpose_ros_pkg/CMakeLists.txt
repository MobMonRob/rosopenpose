cmake_minimum_required(VERSION 2.8.3)
project(openpose_ros_pkg)

set(OPENPOSE_INCLUDE_DIRS ../openpose/include 
						  ../openpose/src 
						  ../openpose/3rdparty/caffe/distribute/include
						  /usr/local/cuda/include)
add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
	std_msgs
	sensor_msgs
	roscpp
	cv_bridge
	openpose_ros_msgs
	roslib
	image_transport
)

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
  CATKIN_DEPENDS std_msgs 
  				 roscpp
  				 cv_bridge
			 	 openpose_ros_msgs  	
			 	 roslib
			 	 image_transport			 
)

###########
## Build ##
###########
include_directories(
# include
  ${catkin_INCLUDE_DIRS}
  ${OPENPOSE_INCLUDE_DIRS}
)

add_definitions(-DUSE_CAFFE)

FIND_LIBRARY(OPENPOSE_LIBRARY openpose /usr/local/lib)
FIND_LIBRARY(CAFFE_LIBRARY caffe /usr/local/lib)
FIND_LIBRARY(CUDA_LIBRARY cuda ../openpose/build/lib)
FIND_LIBRARY(GFLAGS_LIBRARY gflags /usr/lib/x86_64-linux-gnu)
FIND_LIBRARY(GLOG_LIBRARY glog /usr/lib/x86_64-linux-gnu)

add_executable(openpose_ros src/openpose.cpp)
target_link_libraries(openpose_ros ${catkin_LIBRARIES} 
													${OPENPOSE_LIBRARY} 
													${CAFFE_LIBRARY} 
													${CUDA_LIBRARY} 
													${GFLAGS_LIBRARY} 
													${GLOG_LIBRARY})

#add_executable(openpose_ros_node src/openpose_ros_node.cpp)
#target_link_libraries(openpose_ros_node ${catkin_LIBRARIES} 
#										${OPENPOSE_LIBRARY}
#										${CAFFE_LIBRARY} 
#										${CUDA_LIBRARY} 
#										)							
#add_dependencies(openpose_ros_node ${catkin_EXPORTED_TARGETS})

add_executable(openpose_ros_node_3d src/openpose_ros_node_3d.cpp)
target_link_libraries(openpose_ros_node_3d ${catkin_LIBRARIES} 
										${OPENPOSE_LIBRARY}
										${CAFFE_LIBRARY} 
										${CUDA_LIBRARY} 
										)							
add_dependencies(openpose_ros_node_3d ${catkin_EXPORTED_TARGETS})


#add_executable(openpose_ros_node_firephinx src/openpose_ros_node_firephinx.cpp)
#target_link_libraries(openpose_ros_node_firephinx ${catkin_LIBRARIES} 
#										${OPENPOSE_LIBRARY}
#										${CAFFE_LIBRARY}
#										${CUDA_LIBRARY} #										 
#										${GFLAGS_LIBRARY} 
#										${GLOG_LIBRARY})						
#add_dependencies(openpose_ros_node_firephinx ${catkin_EXPORTED_TARGETS})


add_executable(test_openpose_ros_service_call src/test_openpose_ros_service_call.cpp) 
target_link_libraries(test_openpose_ros_service_call ${catkin_LIBRARIES})
add_dependencies(test_openpose_ros_service_call ${catkin_EXPORTED_TARGETS})
