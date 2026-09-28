find_package(Protobuf REQUIRED)

set(PROTOBUF_IMPORT_DIRS)
foreach(ITR ${GAZEBO_INCLUDE_DIRS})
  if(ITR MATCHES ".*gazebo-[0-9.]+$")
    set(PROTOBUF_IMPORT_DIRS "${ITR}/gazebo/msgs/proto")
  endif()
endforeach()

set (msgs
  collision_map_request.proto
  #${PROTOBUF_IMPORT_DIRS}/vector2d.proto
  #${PROTOBUF_IMPORT_DIRS}/header.proto
  #${PROTOBUF_IMPORT_DIRS}/time.proto
)
PROTOBUF_GENERATE_CPP(PROTO_SRCS PROTO_HDRS ${msgs})
add_library(collision_map_creator_msgs SHARED ${PROTO_SRCS})
# Gazebo ships prebuilt vector2d/header/time .pb.h; the generated code includes them by bare name.
target_include_directories(collision_map_creator_msgs PUBLIC ${PROTOBUF_IMPORT_DIRS}/.. ${CMAKE_CURRENT_BINARY_DIR})
target_link_libraries(collision_map_creator_msgs ${PROTOBUF_LIBRARY} ${GAZEBO_LIBRARIES})
