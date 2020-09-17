cmake_minimum_required(VERSION 3.5)

project(tlsf)

if(WIN32 OR APPLE OR ANDROID)
  message(STATUS "tlsf allocator does not target Windows, OSX or Android, skipping...")
  return()
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED)

include_directories(include/tlsf)
add_library(${PROJECT_NAME} STATIC src/tlsf.c src/target.h include/tlsf/tlsf.h)

set_target_properties(${PROJECT_NAME}
  PROPERTIES
    COMPILE_FLAGS "-fvisibility=hidden"
)

ament_export_include_directories(include)
ament_export_libraries(${PROJECT_NAME})

# Disable linting for now.
#if(BUILD_TESTING)
#  find_package(ament_lint_auto REQUIRED)
#  ament_lint_auto_find_test_dependencies()
#endif()

ament_package()

install(DIRECTORY include/
  DESTINATION include
)

install(
  TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)
