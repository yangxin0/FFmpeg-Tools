cmake_minimum_required(VERSION 3.12)
project(FFmpeg-Tools)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(SOURCES_DIR src)
file(GLOB SOURCES ${SOURCES_DIR}/*.c)

find_package(PkgConfig REQUIRED)
pkg_check_modules(LIBAVFORMAT REQUIRED IMPORTED_TARGET libavformat)
pkg_check_modules(LIBAVCODEC REQUIRED IMPORTED_TARGET libavcodec)

foreach(SOURCE ${SOURCES})
    get_filename_component(EXECUTABLE_NAME ${SOURCE} NAME_WE)
    add_executable(${EXECUTABLE_NAME} ${SOURCE})
    target_link_libraries(${EXECUTABLE_NAME} PRIVATE
        PkgConfig::LIBAVFORMAT
        PkgConfig::LIBAVCODEC
    )
    target_include_directories(${EXECUTABLE_NAME} PRIVATE
        ${LIBAVFORMAT_INCLUDE_DIRS}
        ${LIBAVCODEC_INCLUDE_DIRS}
    )
endforeach()
