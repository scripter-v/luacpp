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

get_filename_component(TOP_DIR "${CMAKE_CURRENT_SOURCE_DIR}" PATH)
set(SRC "${TOP_DIR}/src")

if (NOT LUACPP_LUA_LIBRARIES)
  find_package(Lua51)
  if (NOT LUA_LIBRARIES)
    message( FATAL_ERROR "Please set LUACPP_LUA_LIBRARIES variable to your Lua library file. For example:\n cmake -DLUACPP_LUA_LIBRARIES=c:\\lua\\lua5.1.lib ...")
  endif()
  set(LUACPP_LUA_LIBRARIES ${LUA_LIBRARIES})
endif()
message ("Lua library: ${LUACPP_LUA_LIBRARIES}")

if (NOT LUACPP_LUA_INCLUDE_DIR)
  find_package(Lua51)
  if (NOT LUA_INCLUDE_DIR)
    message( FATAL_ERROR "Please set LUACPP_LUA_INCLUDE_DIR variable to the path to your lua.h file. For example:\n cmake -DLUACPP_LUA_INCLUDE_DIR=c:\\lua\\include ...")
  endif()
  set(LUACPP_LUA_INCLUDE_DIR ${LUA_INCLUDE_DIR})
endif()
message ("Lua include dir: ${LUACPP_LUA_INCLUDE_DIR}")

include_directories(
  ${TOP_DIR}/include
  ${LUACPP_LUA_INCLUDE_DIR}
)

set(TEST_SOURCES
  luacpp_test.cpp)

add_executable(luacpp_test
  ${TEST_SOURCES})

target_compile_definitions(luacpp_test PUBLIC "CATCH_CONFIG_MAIN")

target_link_libraries(luacpp_test ${LUACPP_LUA_LIBRARIES})
target_link_libraries(luacpp_test Catch2::Catch2)

add_test(CommonTests luacpp_test)
