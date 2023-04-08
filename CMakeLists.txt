cmake_minimum_required(VERSION 3.18)

SET(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -static")

project("raylib-playground")


add_library(raylib STATIC IMPORTED)

set_property(
  TARGET raylib APPEND PROPERTY IMPORTED_CONFIGURATIONS RELEASE
)

set_target_properties(raylib PROPERTIES
  IMPORTED_LINK_INTERFACE_LANGUAGES_RELEASE "CXX"
  IMPORTED_LOCATION_RELEASE "${raylib_ROOT}/lib/libraylib.a"
)
  
set_target_properties(raylib PROPERTIES
  MAP_IMPORTED_CONFIG_MINSIZEREL Release
  MAP_IMPORTED_CONFIG_RELWITHDEBINFO Release
)

target_include_directories(raylib INTERFACE "${raylib_ROOT}/include")

add_executable(${PROJECT_NAME}
    "src/main.cpp"
)

add_custom_command(
  TARGET ${PROJECT_NAME} POST_BUILD
  COMMAND ${CMAKE_COMMAND} -E copy_directory "${PROJECT_SOURCE_DIR}/resources/" $<TARGET_FILE_DIR:${PROJECT_NAME}>/resources
)

target_link_libraries(${PROJECT_NAME} raylib winmm.lib)

