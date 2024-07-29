cmake_minimum_required(VERSION 3.5)
project(libiscsi C)

include(CheckIncludeFile)
include(CheckSymbolExists)
include(CheckCSourceCompiles)

add_definitions(-DHAVE_CONFIG_H)

execute_process(COMMAND ./autogen.sh
    WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}
)
execute_process(COMMAND ./configure
    WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}
)

include_directories(${PROJECT_SOURCE_DIR}/include)
include_directories(${PROJECT_SOURCE_DIR})

###########################################################################
file(GLOB libiscsi_SRCS lib/*.c)
add_library(libiscsi_static STATIC ${libiscsi_SRCS})
set_target_properties(libiscsi_static PROPERTIES OUTPUT_NAME libiscsi)

###########################################################################
file(GLOB libiscsi_test_tool_SRCS test-tool/*.c)
add_library(libiscsi_test_tool_static STATIC ${libiscsi_test_tool_SRCS})
set_target_properties(libiscsi_test_tool_static PROPERTIES OUTPUT_NAME libiscsi_test_tool)

#############################################################################
install(TARGETS libiscsi_static ARCHIVE DESTINATION lib)
file(GLOB LIBISCSI_HEADERS include/*.h)

file(GLOB LIBISCSI_TEST_HEADERS test-tool/*.h)
install(FILES
    config.h
    ${LIBISCSI_TEST_HEADERS}
    ${LIBISCSI_HEADERS}
    DESTINATION include
)
