cmake_minimum_required(VERSION 3.14)

project(main C)

add_library(main_lib_impl OBJECT source/lib.impl.c)
target_include_directories(
    main_lib_impl PUBLIC
    "$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/source>"
)
target_compile_definitions(main_lib_impl PRIVATE RESULT=0)

add_library(main_lib OBJECT source/lib.c)
target_include_directories(
    main_lib PUBLIC
    "$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/source>"
)
target_link_libraries(
    main_lib
    # This alone should work
    PUBLIC main_lib_impl
    # This is needed for the 'main_exe' target to compile
    # INTERFACE "$<TARGET_OBJECTS:main_lib_impl>"
)

add_executable(main_exe source/main.c)
target_link_libraries(main_exe PRIVATE main_lib)
