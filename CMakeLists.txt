cmake_minimum_required(VERSION 3.16)
project(NewProject)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${NewProject_SOURCE_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${NewProject_SOURCE_DIR}/lib)

add_custom_command(
  OUTPUT index.h 
  COMMAND python ${NewProject_SOURCE_DIR}/A/preparing.py ${NewProject_SOURCE_DIR}
  DEPENDS A/preparing.py
  COMMENT "Generating file from json"
)


add_subdirectory(B)
add_custom_target(Generator DEPENDS index.h COMMENT "Checking if re-generation is required")

add_executable(NewProject C/main.cpp)

add_dependencies(NewProject Generator)

target_link_libraries(NewProject library)
