cmake_minimum_required(VERSION 3.13 FATAL_ERROR)

project(emulatorapi)

file(GLOB BN_EMULATOR_API_SOURCES CONFIGURE_DEPENDS *.cpp *.h)
add_library(emulatorapi STATIC ${BN_EMULATOR_API_SOURCES})

target_include_directories(emulatorapi
		PUBLIC ${PROJECT_SOURCE_DIR})

target_link_libraries(emulatorapi PUBLIC emulatorcore)

set_target_properties(emulatorapi PROPERTIES
		CXX_STANDARD 20
		CXX_VISIBILITY_PRESET hidden
		CXX_STANDARD_REQUIRED ON
		VISIBILITY_INLINES_HIDDEN ON
		POSITION_INDEPENDENT_CODE ON
		ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/out)

if (NOT DEMO)
	add_subdirectory(python)
endif()
