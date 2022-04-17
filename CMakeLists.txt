cmake_minimum_required (VERSION 3.10) 
project (raspicam_cpp 
    VERSION 0.3.0
    LANGUAGES CXX
)
set (CMAKE_CXX_STANDARD 17)

# Determine whether this is a standalone project or included by other projects
set(CXXOPTS_STANDALONE_PROJECT OFF)
if (CMAKE_CURRENT_SOURCE_DIR STREQUAL CMAKE_SOURCE_DIR)
    set(CXXOPTS_STANDALONE_PROJECT ON)
endif()

option(RASPICAM_CPP_BUILD_EXAMPLES "Set to ON to build examples" ${CXXOPTS_STANDALONE_PROJECT})




# ===== Required packages/ dependencies ===== 
#find_package(OpenCV REQUIRED)
#message( "Open CV version: ${OpenCV_VERSION}" )

#find_package(cxxopts REQUIRED)  # for test main
#find_package(spdlog REQUIRED)
if(CXXOPTS_STANDALONE_PROJECT)
	add_subdirectory(lib/cxxopts)
	add_subdirectory(lib/spdlog)
endif()

# ===== Compile settings =====
add_compile_options(-Wall -Wextra -pedantic)

# ===== Costum libraries =====
add_library(bcm_host IMPORTED SHARED)
  #PATHS /opt/vc/lib/libbcm_host.so)
target_include_directories(bcm_host
  INTERFACE
	/opt/vc/include
)
target_link_directories(bcm_host
  INTERFACE
	/opt/vc/lib
)
set_target_properties(bcm_host PROPERTIES IMPORTED_LOCATION /opt/vc/lib/libbcm_host.so )

add_library(mmal IMPORTED SHARED)
target_include_directories(mmal
  INTERFACE
	/opt/vc/include/interface/mmal
	/opt/vc/include     # for vcos required by mmal_common.h
)

set_target_properties(mmal PROPERTIES IMPORTED_LOCATION /opt/vc/lib/libmmal.so )

# Not currently directly needed
add_library(vcos IMPORTED SHARED)
target_include_directories(vcos
  INTERFACE
	/opt/vc/include/interface/vcos
)
set_target_properties(vcos PROPERTIES IMPORTED_LOCATION /opt/vc/lib/libvcos.so )


add_library(${PROJECT_NAME}
	src/raspi_encamode.cpp
	src/raspicam_camcontrol.cpp
)
target_include_directories(${PROJECT_NAME}
  PUBLIC
	include/
  PRIVATE
)
target_link_libraries(${PROJECT_NAME}
  PUBLIC
	mmal
  PRIVATE
	spdlog
	bcm_host
)

# ===== Executables =====
if (CXXOPTS_BUILD_EXAMPLES)
	add_executable(${PROJECT_NAME}_test
		src/raspi_encamode_testmain.cpp)
	target_link_libraries(${PROJECT_NAME}_test
	PRIVATE
		cxxopts::cxxopts
		${PROJECT_NAME}
	)
endif()


