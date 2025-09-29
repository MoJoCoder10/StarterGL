cmake_minimum_required(VERSION 3.10)
project(SnakeMacOS LANGUAGES CXX C)

# Compiler flags
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Silence OpenGL deprecation warnings on macOS
add_definitions(-DGL_SILENCE_DEPRECATION)

# Include directories
include_directories(
    include
    /usr/local/include
)

# Source files
set(SOURCES
    src/main.cpp
    src/glad.c
)

# Create the executable
add_executable(SnakeMacOS ${SOURCES})

# Link libraries (macOS frameworks need special handling)
find_package(OpenGL REQUIRED)

target_link_libraries(SnakeMacOS
    glfw
    dl
    ${OPENGL_gl_LIBRARY}
)

# On macOS, explicitly link OpenGL framework
if(APPLE)
    target_link_libraries(SnakeMacOS "-framework OpenGL")
endif()

# Output directory (bin/)
set_target_properties(SnakeMacOS PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin
)

# Optional: custom command to copy shaders next to binary
# add_custom_command(TARGET SnakeMacOS POST_BUILD
#     COMMAND ${CMAKE_COMMAND} -E copy_directory
#     ${CMAKE_SOURCE_DIR}/shaders $<TARGET_FILE_DIR:SnakeMacOS>/shaders
# )
