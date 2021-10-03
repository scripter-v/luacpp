cmake_minimum_required(VERSION 3.15)

project(luacpp_test)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

include( FetchContent )

FetchContent_Declare(
    Catch2
    GIT_REPOSITORY https://github.com/catchorg/Catch2.git
    GIT_TAG        v2.13.7
    )

FetchContent_MakeAvailable(Catch2)

find_package(Lua REQUIRED)
message ("Lua library: ${LUA_LIBRARIES}")
message ("Lua include dir: ${LUA_INCLUDE_DIR}")
message ("Lua version: ${LUA_VERSION_STRING}")

get_filename_component(TOP_DIR "${CMAKE_CURRENT_SOURCE_DIR}" PATH)
set(SRC "${TOP_DIR}/src")

include_directories(
  ${TOP_DIR}/include
  ${LUA_INCLUDE_DIR}
)

set(TEST_SOURCES
  luacpp_test.cpp)

add_executable(luacpp_test
  ${TEST_SOURCES})

target_compile_definitions(luacpp_test PUBLIC "CATCH_CONFIG_MAIN")

target_link_libraries(luacpp_test ${LUA_LIBRARIES})
target_link_libraries(luacpp_test Catch2::Catch2)

add_test(CommonTests luacpp_test)
