cmake_minimum_required(VERSION 3.11)
project(txdib LANGUAGES C CXX)
include(FetchContent)

# Grab and build libs
FetchContent_Declare(freeimage GIT_REPOSITORY https://github.com/tordex/freeimage)
if(NOT freeimage_POPULATED)
	FetchContent_Populate(freeimage)
	add_library(freeimage SHARED IMPORTED)
	set_target_properties(freeimage PROPERTIES IMPORTED_LOCATION ${freeimage_SOURCE_DIR}/64/freeimage.lib)
	set_target_properties(freeimage PROPERTIES IMPORTED_IMPLIB   ${freeimage_SOURCE_DIR}/64/freeimage.lib)
	target_include_directories(freeimage INTERFACE ${freeimage_SOURCE_DIR})
endif()


# build
set(SOURCE_TXDIB
    TxDIB.cpp
)

set(HEADER_TXDIB
    TxDIB.h
)

add_library(${PROJECT_NAME} ${SOURCE_TXDIB})

# Link to dependencies
target_link_libraries(${PROJECT_NAME}
	PRIVATE freeimage
)

set_target_properties(${PROJECT_NAME} PROPERTIES
    CXX_STANDARD 11
    C_STANDARD 99
    PUBLIC_HEADER "${HEADER_TXDIB}"
)

# Export litehtml includes.
target_include_directories(${PROJECT_NAME} PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include/${PROJECT_NAME}>)
target_include_directories(${PROJECT_NAME} PRIVATE include/${PROJECT_NAME})

# install and export
install(TARGETS ${PROJECT_NAME}
    EXPORT txdibTargets
    RUNTIME DESTINATION bin COMPONENT libraries
    ARCHIVE DESTINATION lib${LIB_SUFFIX} COMPONENT libraries
    LIBRARY DESTINATION lib${LIB_SUFFIX} COMPONENT libraries
    PUBLIC_HEADER DESTINATION include/txdib
)
install(FILES cmake/txdibConfig.cmake DESTINATION lib${LIB_SUFFIX}/cmake/txdib)
install(EXPORT txdibTargets FILE txdibTargets.cmake DESTINATION lib${LIB_SUFFIX}/cmake/txdib)
