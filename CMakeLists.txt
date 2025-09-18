cmake_minimum_required(VERSION 3.16)
project(Rasterizer)

add_subdirectory(deps/glm)

find_package(SDL2 REQUIRED)
find_package(TINYOBJLOADER REQUIRED)


add_executable(Rasterizer main.cpp Texture.cpp)

target_include_directories(Rasterizer PRIVATE
    deps/stb
    deps/glm
    ${SDL2_INCLUDE_DIRS}
    ${TINYOBJLOADER_INCLUDE_DIRS}
)


target_link_libraries(Rasterizer 	
	${SDL2_LIBRARIES}
)

set(OuputFile "$<TARGET_FILE_DIR:Rasterizer>")


# Where the exe will live
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR}/bin)

# Define the install layout (like QMake does automatically)
install(TARGETS Rasterizer
    RUNTIME DESTINATION bin
)

# Copy SDL2 DLL next to Rasterizer.exe after build
add_custom_command(TARGET Rasterizer POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        $<TARGET_FILE:SDL2::SDL2>       # SDL2.dll (resolved by CMake)
        $<TARGET_FILE_DIR:Rasterizer>   # copy into exe dir
)

# Do the same for other DLLs if needed (example: SDL2_image, tinyobjloader if built as shared lib, etc.)
# add_custom_command(TARGET Rasterizer POST_BUILD
#     COMMAND ${CMAKE_COMMAND} -E copy_if_different
#         $<TARGET_FILE:SDL2_image::SDL2_image>
#         $<TARGET_FILE_DIR:Rasterizer>
# )

