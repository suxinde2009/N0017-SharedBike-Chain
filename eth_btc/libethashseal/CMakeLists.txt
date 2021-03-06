aux_source_directory(. SRC_LIST)

file(GLOB HEADERS "*.h")

add_subdirectory(genesis)
aux_source_directory(./genesis SRCS)
set(SRC_LIST ${SRC_LIST} ${SRCS})

if ( CMAKE_COMPILER_IS_GNUCC )
    set(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} -Wno-error=unused-parameter")
endif ( CMAKE_COMPILER_IS_GNUCC )
if ( MSVC )
    set(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} /W4")
endif ( MSVC )

add_library(ethashseal ${SRC_LIST} ${HEADERS})
eth_use(ethashseal OPTIONAL Eth::ethash-cl Cpuid)

find_package(Eth)

target_include_directories(ethashseal PRIVATE ..)
target_link_libraries(ethashseal ${Eth_ETHASH_LIBRARIES})
target_link_libraries(ethashseal ${Eth_ETHEREUM_LIBRARIES})
