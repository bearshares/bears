
add_library( bears_jsonball
   mm_ticks.cpp
   rc_params.cpp
   ${CMAKE_CURRENT_SOURCE_DIR}/include/bears/jsonball/jsonball.hpp
)

target_link_libraries( bears_jsonball fc )

target_include_directories( bears_jsonball PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include" )

INSTALL( TARGETS
   bears_jsonball

   RUNTIME DESTINATION bin
   LIBRARY DESTINATION lib
   ARCHIVE DESTINATION lib
)