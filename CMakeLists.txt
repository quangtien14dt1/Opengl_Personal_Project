cmake_minimum_required(VERSION 3.10)

#Set up project
project(OPenGl_learning VERSION 1.0)

# Configure assets header file
configure_file(src/helpers/RootDir.h.in src/helpers/RootDir.h)

# Define the include DIRs
include_directories(
	"${CMAKE_SOURCE_DIR}/src"
	"${CMAKE_SOURCE_DIR}/include"
	"${CMAKE_SOURCE_DIR}/include/glad"
	"${CMAKE_SOURCE_DIR}/include/glm"
)

#Set executable
add_executable(OPenGl_learning
    Source/Item/Material.cpp
    Source/Item/ItemStack.cpp
    Source/Application.cpp
    Source/World/Event/PlayerDigEvent.cpp
    Source/World/Block/BlockDatabase.cpp
    Source/World/Block/BlockData.cpp
    Source/World/Block/ChunkBlock.cpp
    Source/World/Block/BlockTypes/BlockType.cpp
    Source/World/World.cpp
    Source/World/Generation/Biome/LightForest.cpp
    Source/World/Generation/Biome/Biome.cpp
    Source/World/Generation/Biome/DesertBiome.cpp
    Source/World/Generation/Biome/GrasslandBiome.cpp
    Source/World/Generation/Biome/TemperateForestBiome.cpp
    Source/World/Generation/Biome/OceanBiome.cpp
    Source/World/Generation/Structures/TreeGenerator.cpp
    Source/World/Generation/Structures/StructureBuilder.cpp
    Source/World/Generation/Terrain/SuperFlatGenerator.cpp
    Source/World/Generation/Terrain/ClassicOverWorldGenerator.cpp
    Source/World/Chunk/ChunkMesh.cpp
    Source/World/Chunk/ChunkManager.cpp
    Source/World/Chunk/Chunk.cpp
    Source/World/Chunk/ChunkSection.cpp
    Source/World/Chunk/ChunkMeshBuilder.cpp
    Source/States/PlayingState.cpp
    Source/Physics/AABB.cpp
    Source/Player/Player.cpp
    Source/Maths/Ray.cpp
    Source/Maths/Frustum.cpp
    Source/Maths/NoiseGenerator.cpp
    Source/Maths/Vector2XZ.cpp
    Source/Maths/Matrix.cpp
    Source/Maths/GeneralMaths.cpp
    Source/Camera.cpp
    Source/GL/GLFunctions.cpp
    Source/Context.cpp
    Source/Texture/CubeTexture.cpp
    Source/Texture/BasicTexture.cpp
    Source/Texture/TextureAtlas.cpp
    Source/Input/ToggleKey.cpp
    Source/Input/Keyboard.cpp
    Source/Main.cpp
    Source/Controller.cpp
    Source/Util/Random.cpp
    Source/Util/FPSCounter.cpp
    Source/Util/FileUtil.cpp
    Source/Shaders/FloraShader.cpp
    Source/Shaders/WaterShader.cpp
    Source/Shaders/SkyboxShader.cpp
    Source/Shaders/Shader.cpp
    Source/Shaders/ChunkShader.cpp
    Source/Shaders/BasicShader.cpp
    Source/Shaders/ShaderLoader.cpp
    Source/Renderer/RenderMaster.cpp
    Source/Renderer/WaterRenderer.cpp
    Source/Renderer/ChunkRenderer.cpp
    Source/Renderer/SkyboxRenderer.cpp
    Source/Renderer/SFMLRenderer.cpp
    Source/Renderer/FloraRenderer.cpp
    Source/Model.cpp
)

#Set module path ${GLM_INCLUDE_DIR} ${SFML_LIBRARIES}
set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake_modules" ${CMAKE_MODULE_PATH})

# Thread
find_package(Threads)

# SFML
find_package(SFML REQUIRED audio network graphics window system)

# OpenGL
set(OpenGL_GL_PREFERENCE GLVND)
find_package(OpenGL REQUIRED)

# GLFW
find_package(GLFW3 REQUIRED)
message(STATUS "Found GLFW3 in ${GLFW3_INCLUDE_DIR}")

# ASSIMP
find_package(ASSIMP REQUIRED)
message(STATUS "Found ASSIMP in ${ASSIMP_INCLUDE_DIR}")

# STB_IMAGE
add_library(STB_IMAGE "thirdparty/stb_image.cpp")

# subdirectory
add_subdirectory(deps/glm)
add_subdirectory(deps/glad)



function(setup_target TARGET)
    #Set C++17
    target_compile_features(${TARGET} PUBLIC cxx_std_17)
    set_target_properties(${TARGET} PROPERTIES CXX_EXTENSIONS OFF)

    #Set flags
    if(MSVC)
        target_compile_options(${TARGET} PRIVATE
            /W4 /WX)
    else()
        target_compile_options(${TARGET} PRIVATE
            -Wall -Wextra -pedantic)
    endif()

    target_include_directories(
        ${TARGET}
        PRIVATE
        deps
    )
endfunction(setup_target)

setup_target(OPenGl_learning)

# Add .lib files
link_directories(${CMAKE_SOURCE_DIR}/lib)

# target link static libs
target_link_libraries(OPenGl_learning
    glm
    glad
    Threads::Threads
    STB_IMAGE
    ${SFML_LIBRARIES}
    ${SFML_DEPENDENCIES}
    ${CMAKE_DL_LIBS}
    ${GLFW3_LIBRARY}
    ${OPENGL_LIBRARY}
    ${ASSIMP_LIBRARY}
)
