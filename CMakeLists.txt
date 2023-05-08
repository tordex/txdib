cmake_minimum_required(VERSION 3.11)
project(txdib LANGUAGES C CXX)

set(SOURCE_TXDIB
    TxDIB.cpp
)

set(HEADER_TXDIB
    TxDIB.h
)

add_library(${PROJECT_NAME} ${SOURCE_TXDIB})

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
