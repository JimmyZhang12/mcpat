add_subdirectory(cacti)
add_subdirectory(cache)
add_subdirectory(core)
add_subdirectory(iocontrollers)
add_subdirectory(logic)
add_subdirectory(memoryctrl)

add_library(top
  arch_const.h
  array.h
  array.cc
  basic_components.h
  basic_components.cc
  globalvar.h
  interconnect.h
  interconnect.cc
  noc.h
  noc.cc
  options.h
  options.cc
  processor.h
  processor.cc
  version.h
  xmlParser.h
  xmlParser.cc
  XML_Parse.h
  XML_Parse.cc
)
target_include_directories(top PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
target_link_libraries(top
                      LINK_PUBLIC
                      cache
                      cacti
                      core
                      logic
                      iocontrollers
                      memoryctrl)

add_executable(mcpat main.cc)

target_link_libraries(mcpat
                      LINK_PUBLIC
                      top
                      Threads::Threads
                      Boost::program_options
                      Boost::serialization)

add_custom_command(TARGET mcpat POST_BUILD
  COMMAND ${CMAKE_COMMAND} -E copy $<TARGET_FILE:mcpat> ${PROJECT_BINARY_DIR}/mcpat
  COMMENT "Copying executable ${PROJECT_BINARY_DIR}/mcpat"
)
