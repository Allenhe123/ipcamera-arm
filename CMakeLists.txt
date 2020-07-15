cmake_minimum_required(VERSION 3.10)

project(ipcamera)

add_definitions(-std=c++14)

find_package(OpenCV REQUIRED)

if (NOT OpenCV_FOUND)
    message(FATAL_ERROR "opencv Not Found!")
else ()
    message(STATUS "OpenCV_LIBS: ${OpenCV_LIBS}")
    message(STATUS "OpenCV_INCLUDE_DIRS: ${OpenCV_INCLUDE_DIRS}")
endif (NOT OpenCV_FOUND)
include_directories(${OpenCV_INCLUDE_DIRS})

include_directories(onvif)
# include_directories(/usr/local/ffmpeg/include)

add_subdirectory(onvif)

set(ffmpeglibs libavcodec.so 
    libswscale.so
    libavfilter.so
    libavdevice.so
    libavformat.so
    libavutil.so
    libswresample.so)

add_executable(ipcamera camera-test.cpp onvifpipe.cpp decodethread.cpp)
target_link_libraries(ipcamera onvif ssl crypto ${ffmpeglibs} ${OpenCV_LIBS} pthread)