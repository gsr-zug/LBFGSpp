cmake_minimum_required(VERSION 3.18)
project(LBFGSpp VERSION 0.1 LANGUAGES CXX)

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

add_library(${CMAKE_PROJECT_NAME} INTERFACE)
target_compile_features(${PROJECT_NAME} INTERFACE cxx_std_11)
target_include_directories(${CMAKE_PROJECT_NAME} INTERFACE
    $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)
target_link_libraries(${CMAKE_PROJECT_NAME} INTERFACE Eigen3::Eigen)

install(TARGETS ${CMAKE_PROJECT_NAME}
        EXPORT exportedTargets
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR} FILES_MATCHING PATTERN "*.h")

set(INSTALL_CONFIG_DIR ${CMAKE_INSTALL_LIBDIR}/cmake/${CMAKE_PROJECT_NAME})
install(EXPORT exportedTargets FILE ${CMAKE_PROJECT_NAME}Targets.cmake DESTINATION ${INSTALL_CONFIG_DIR})
write_basic_package_version_file(${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_PROJECT_NAME}ConfigVersion.cmake COMPATIBILITY SameMajorVersion)
configure_package_config_file(
        ${CMAKE_CURRENT_LIST_DIR}/cmake/${CMAKE_PROJECT_NAME}Config.cmake.in
        ${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_PROJECT_NAME}Config.cmake
        INSTALL_DESTINATION ${INSTALL_CONFIG_DIR})
install(FILES
        ${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_PROJECT_NAME}Config.cmake
        ${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_PROJECT_NAME}ConfigVersion.cmake
        DESTINATION ${INSTALL_CONFIG_DIR})

# Exporting from the build tree
export(EXPORT exportedTargets FILE ${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_PROJECT_NAME}Targets.cmake)
export(PACKAGE ${CMAKE_PROJECT_NAME})
