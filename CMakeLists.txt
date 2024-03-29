cmake_minimum_required(VERSION 3.21)
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

##
# Function that generates source groups from directories
##
function(GenerateSourceGoups Sources)
	foreach(Source ${Sources})
		get_filename_component(SourcePath "${Source}" PATH)
		if (WIN32)
			string(REPLACE "/" "\\" SourcePath "${SourcePath}")
		endif()
		source_group("${SourcePath}" FILES "${Source}")
	endforeach()
endfunction()

##
# Start
##
project(s4_fs VERSION "1.0.0")
enable_testing()

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++2a")

set(S4_FS_PROJECT_ROOT_DIR ${CMAKE_SOURCE_DIR})
set(S4_FS_PROJECT_DIST_DIR "${S4_FS_PROJECT_ROOT_DIR}/dist")
set(S4_FS_PROJECT_COMMON_BUILD_DIR "${S4_FS_PROJECT_ROOT_DIR}/common_build")

if(MSVC)
add_compile_options(/W4 /WX)
else()
add_compile_options(-Wall -Wextra -Wpedantic -Werror -Wno-unused-variable)
endif()

# Output dirs
if (WIN32)
	set(S4_FS_PROJECT_FULL_DIST_DIR "${S4_FS_PROJECT_DIST_DIR}") # Config name is added autmatically on windows
else()
	set(S4_FS_PROJECT_FULL_DIST_DIR "${S4_FS_PROJECT_DIST_DIR}/${BUILD_CONFIG_NAME}")
endif()
	
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${S4_FS_PROJECT_FULL_DIST_DIR})
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${S4_FS_PROJECT_FULL_DIST_DIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${S4_FS_PROJECT_FULL_DIST_DIR})

include(FetchContent)
FetchContent_Declare(
  s4_rtti
  GIT_REPOSITORY "https://github.com/PiotrMoscicki/s4_rtti"
  GIT_TAG "main"
)

FetchContent_MakeAvailable(s4_rtti)

# Target names
set(S4_FS_TARGET s4_fs)

# Subdirectories
add_subdirectory(src)