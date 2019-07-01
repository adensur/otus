cmake_minimum_required(VERSION 3.8)
project(helloworld VERSION 0.0.$ENV{TRAVIS_BUILD_NUMBER})

find_package(Boost COMPONENTS unit_test_framework REQUIRED)

add_executable(helloworld main.cpp)
add_executable(test_version test_version.cpp)

set_target_properties(helloworld test_version PROPERTIES
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED ON    
)

configure_file(version.h.in version.h)
target_include_directories(helloworld
    PRIVATE "${CMAKE_BINARY_DIR}"
)

target_link_libraries(test_version
    ${Boost_LIBRARIES}
)

set_target_properties(test_version PROPERTIES
    COMPILE_DEFINITIONS BOOST_TEST_DYN_LINK
    INCLUDE_DIRECTORIES ${Boost_INCLUDE_DIR}
)

target_include_directories(test_version
    PRIVATE "${CMAKE_BINARY_DIR}"
)