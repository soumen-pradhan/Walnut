cmake_minimum_required(VERSION 3.24)
project(Walnut LANGUAGES CXX)

add_library(${PROJECT_NAME} "")
add_library(${PROJECT_NAME}::${PROJECT_NAME} ALIAS ${PROJECT_NAME})

set_target_properties(${PROJECT_NAME}
    PROPERTIES
        CXX_STANDARD 17
        CXX_STANDARD_REQUIRED ON
        CXX_EXTENSIONS OFF
)

target_sources(${PROJECT_NAME}
    PUBLIC
        FILE_SET walnut_headers
        TYPE HEADERS
        BASE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}/src
        FILES
            src/Walnut/Application.h
            src/Walnut/EntryPoint.h
            src/Walnut/Image.h
            src/Walnut/Layer.h
            src/Walnut/Random.h
            src/Walnut/Timer.h
            src/Walnut/Input/Input.h
            src/Walnut/Input/KeyCodes.h

    PRIVATE
        FILE_SET walnut_fonts
        TYPE HEADERS
        BASE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}/src
        FILES src/Walnut/ImGui/Roboto-Regular.embed

    PRIVATE
        src/Walnut/Application.cpp
        src/Walnut/Image.cpp
        src/Walnut/Random.cpp
        src/Walnut/ImGui/ImGuiBuild.cpp
        src/Walnut/Input/Input.cpp
)

target_link_libraries(${PROJECT_NAME}
    PUBLIC
        glm::glm
        imgui::imgui
        stb::stb
)

# Windows-specific settings
if(WIN32)
    target_compile_definitions(${PROJECT_NAME} PUBLIC
        WL_PLATFORM_WINDOWS
    )
endif()

target_compile_definitions(${PROJECT_NAME} PUBLIC
    $<$<CONFIG:Debug>:WL_DEBUG>
    $<$<CONFIG:RelWithDebInfo>:WL_RELEASE>
    $<$<CONFIG:Release>:WL_DIST>
)
