file(GLOB HEADERS "include/bears/chain/*.hpp" "include/bears/chain/util/*.hpp")

## SORT .cpp by most likely to change / break compile
add_library( bears_chain

             # As database takes the longest to compile, start it first
             database.cpp

             smt_evaluator.cpp
             smt_objects/smt_market_maker.cpp
             bears_evaluator.cpp
             bears_objects.cpp

             witness_schedule.cpp
             fork_database.cpp

             shared_authority.cpp
             block_log.cpp

             generic_custom_operation_interpreter.cpp

             util/rd.cpp
             util/reward.cpp
             util/impacted.cpp
             util/advanced_benchmark_dumper.cpp

             ${HEADERS}
           )

target_link_libraries( bears_chain bears_jsonball bears_protocol fc chainbase bears_schema appbase
                       ${PATCH_MERGE_LIB} )
target_include_directories( bears_chain
                            PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}"
                            PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include" "${CMAKE_CURRENT_BINARY_DIR}/include" )

if( CLANG_TIDY_EXE )
   set_target_properties(
      bears_chain PROPERTIES
      CXX_CLANG_TIDY "${DO_CLANG_TIDY}"
   )
endif( CLANG_TIDY_EXE )

if(MSVC)
  set_source_files_properties( database.cpp PROPERTIES COMPILE_FLAGS "/bigobj" )
endif(MSVC)

INSTALL( TARGETS
   bears_chain

   RUNTIME DESTINATION bin
   LIBRARY DESTINATION lib
   ARCHIVE DESTINATION lib
)
INSTALL( FILES ${HEADERS} DESTINATION "include/bears/chain" )
