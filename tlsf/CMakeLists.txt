cmake_minimum_required(VERSION 2.8.3)

project(tlsf)

if(WIN32 OR APPLE)
  message(STATUS "tlsf allocator does not target Windows or OSX, skipping...")
  return()
endif()

find_package(ament_cmake REQUIRED)

include_directories(include/tlsf)
add_library(${PROJECT_NAME} STATIC src/tlsf.c src/target.h include/tlsf/tlsf.h)

set_target_properties(${PROJECT_NAME}
  PROPERTIES
    COMPILE_FLAGS "-fvisibility=hidden -fvisibility-inlines-hidden"
)

ament_export_include_directories(include)
ament_export_libraries(${PROJECT_NAME})

# Disable linting for now.
#if(AMENT_ENABLE_TESTING)
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
