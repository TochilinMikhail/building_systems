cmake_minimum_required(VERSION 3.16)
project(NewProject)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${NewProject_SOURCE_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${NewProject_SOURCE_DIR}/lib)

find_package(PythonInterp REQUIRED)
add_custom_command(
  OUTPUT index.h 
  COMMAND ${PYTHON_EXECUTABLE} ${NewProject_SOURCE_DIR}/A/preparing.py ${NewProject_SOURCE_DIR}
  DEPENDS A/preparing.py
  COMMENT "Generating file from json"
)


add_subdirectory(B)
add_custom_target(Generator DEPENDS index.h COMMENT "Checking if re-generation is required")


add_executable(NewProject C/main.cpp)

add_dependencies(NewProject Generator)

target_link_libraries(NewProject library)
target_include_directories(NewProject PRIVATE  ${PROJECT_BINARY_DIR} ${PROJECT_SOURCE_DIR})
