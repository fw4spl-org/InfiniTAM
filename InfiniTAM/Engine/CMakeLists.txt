include_directories(${OPENGL_INCLUDE_DIR})

if(WITH_GLUT)
    include_directories(${GLUT_INCLUDE_DIR})
else()
    add_definitions(-DCOMPILE_WITHOUT_GLUT)
endif()

if(WITH_CUDA)
  include_directories(${CUDA_INCLUDE_DIRS})
else()
  add_definitions(-DCOMPILE_WITHOUT_CUDA)
endif()

if(OPENNI_FOUND)
  include_directories(${OpenNI_INCLUDE_DIR})
else()
  add_definitions(-DCOMPILE_WITHOUT_OpenNI)
endif()

if(MSKINECTAPI_FOUND)
  include_directories(${MSKINECTAPI_INCLUDE_DIR})
else()
  add_definitions(-DCOMPILE_WITHOUT_Kinect2)
endif()

if(DEFINED libuvc_INCLUDE_DIRS)
  include_directories(${libuvc_INCLUDE_DIRS})
  add_definitions(-DCOMPILE_WITH_LibUVC)
endif()

if(WITH_REALSENSE)
  include_directories(${RealSense_INCLUDE_DIR})
  add_definitions(-DCOMPILE_WITH_RealSense)
  SET_PROPERTY(SOURCE RealSenseEngine.cpp PROPERTY COMPILE_FLAGS -std=c++11)
endif()

set(ITMLIB_ENGINE_HEADERS
    ImageSourceEngine.h
    IMUSourceEngine.h
    Kinect2Engine.h
    OpenNIEngine.h
    LibUVCEngine.h
    UIEngine.h
    CLIEngine.h
    RealSenseEngine.h
)
set(ITMLIB_ENGINE_SOURCES
    ImageSourceEngine.cpp
    IMUSourceEngine.cpp
    Kinect2Engine.cpp
    OpenNIEngine.cpp
    LibUVCEngine.cpp
    UIEngine.cpp
    CLIEngine.cpp
    RealSenseEngine.cpp
)

add_library(Engine ${ITMLIB_ENGINE_SOURCES} ${ITMLIB_ENGINE_HEADERS})

if(WITH_GLUT)
    target_link_libraries(Engine ${GLUT_LIBRARIES})
endif()
target_link_libraries(Engine ${OPENGL_LIBRARIES})
target_link_libraries(Engine ITMLib)
if(WITH_CUDA)
  target_link_libraries(Engine ${CUDA_LIBRARY})
endif()
if(OPENNI_FOUND)
  target_link_libraries(Engine ${OpenNI_LIBRARY})
endif(OPENNI_FOUND)
if(DEFINED libuvc_LIBRARIES)
  target_link_libraries(Engine ${libuvc_LIBRARIES})
endif()
if(WITH_REALSENSE)
  TARGET_LINK_LIBRARIES(Engine ${RealSense_LIBRARY})
endif()

install(TARGETS Engine
          RUNTIME DESTINATION bin/InfiniTAM
          ARCHIVE DESTINATION lib/InfiniTAM
          LIBRARY DESTINATION lib/InfiniTAM)
install(FILES ${ITMLIB_ENGINE_HEADERS}
        DESTINATION ${CMAKE_INSTALL_PREFIX}/include/InfiniTAM/Engine)
