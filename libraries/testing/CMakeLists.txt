file(GLOB HEADERS "include/eosio/testing/*.hpp")

## SORT .cpp by most likely to change / break compile
add_library( eosio_testing
             tester.cpp
             tester_network.cpp
             ${HEADERS}
        )

target_link_libraries( eosio_testing eosio_chain eos_utilities fc chainbase Logging IR WAST WASM Runtime )
target_include_directories( eosio_testing
                            PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include" "${CMAKE_CURRENT_BINARY_DIR}/include"
                                   "${CMAKE_CURRENT_SOURCE_DIR}/../wasm-jit/Include"
                                   "${CMAKE_BINARY_DIR}/contracts"
                            )

add_dependencies( eosio_testing eosio.bios )

if(MSVC)
  set_source_files_properties( db_init.cpp db_block.cpp database.cpp block_log.cpp PROPERTIES COMPILE_FLAGS "/bigobj" )
endif(MSVC)

