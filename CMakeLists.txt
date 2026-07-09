cmake_minimum_required(VERSION 3.13 FATAL_ERROR)

project(bnil-emulator)

if((NOT BN_API_PATH) AND (NOT BN_INTERNAL_BUILD))
	set(BN_API_PATH $ENV{BN_API_PATH})
	if(NOT BN_API_PATH)
		message(FATAL_ERROR "Provide path to Binary Ninja API source in BN_API_PATH")
	endif()
endif()

add_subdirectory(core)
add_subdirectory(api)
