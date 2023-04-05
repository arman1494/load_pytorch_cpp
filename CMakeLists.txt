cmake_minimum_required(VERSION 3.26.2 FATAL_ERROR)

project(load-pytorch)

set(CMAKE_PREFIX_PATH libtorch)

find_package(Torch REQUIRED)



set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${TORCH_CXX_FLAGS}")

add_executable(load-pytorch load-pytorch.cpp)

target_link_libraries(load-pytorch "${TORCH_LIBRARIES}")

set_property(TARGET load-pytorch PROPERTY CXX_STANDARD 14)

if (MSVC)
  file(GLOB TORCH_DLLS "${TORCH_INSTALL_PREFIX}/lib/*.dll")
  add_custom_command(TARGET load-pytorch
                     POST_BUILD
                     COMMAND ${CMAKE_COMMAND} -E copy_if_different
                     ${TORCH_DLLS}
                     $<TARGET_FILE_DIR:load-pytorch>)
endif (MSVC)