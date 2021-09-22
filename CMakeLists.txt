cmake_minimum_required(VERSION 3.1)

project(toml C)

set(PACKAGE_VERSION 20100204)
option(DEBUG "switch on debug option" OFF)
if (DEBUG)
set(DEBUG_OPTION "-O0 -g")
else()
set(DEBUG_OPTION "-O2 -DNDEBUG")
endif()
set(CMAKE_C_FLAGS "-std=c99 -Wall -Wextra -fpic ${DEBUG_OPTION}")

##libtoml.a
add_library(toml src/toml.c)
target_include_directories(toml PUBLIC
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include> 
    $<INSTALL_INTERFACE:include>
)
set_target_properties(toml PROPERTIES
	PUBLIC_HEADER include/toml.h
)

##toml_json
add_executable(toml_json samples/toml_json.c)

target_link_libraries(toml_json toml)

##toml_cat
add_executable(toml_cat samples/toml_cat.c)

target_link_libraries(toml_cat toml)

##toml_sample
add_executable(toml_sample samples/toml_sample.c)

target_link_libraries(toml_sample toml)

##install
install(TARGETS toml EXPORT tomlExport
	LIBRARY		DESTINATION lib
	INCLUDES	DESTINATION include
	PUBLIC_HEADER	DESTINATION include)
install(EXPORT tomlExport
	FILE toml-config.cmake
	DESTINATION share/cmake/toml
	EXPORT_LINK_INTERFACE_LIBRARIES
)

