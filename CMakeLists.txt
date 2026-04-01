cmake_minimum_required(VERSION 3.16)
project(TemplateSFML)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
option(BUILD_SHARED_LIBS "Build shared libraries" OFF)

#---- Fetch SFML, only works if you have git installed on your machine ----#

include(FetchContent)   # Allows fetching content remotely at configure time
FetchContent_Declare(   # Fetches SFML from github and puts it in the build directory
    SFML
    GIT_REPOSITORY https://github.com/SFML/SFML.git
    GIT_TAG 69ea0cd863aed1d4092b970b676924a716ff718b    # Makes sure to fetch SFML version 2.6.1
)

FetchContent_MakeAvailable(SFML)    # Makes sure SFML and its dependencies are avaiable for the project

#---- File management & .exe creation ----#

#---- Paths ----#
set(SRC_DIR "${PROJECT_SOURCE_DIR}/src")  # Sets variable SRC_DIR to the path of the source directory
set(INC_DIR "${PROJECT_SOURCE_DIR}/include")
set(ASSETS_DIR "${PROJECT_SOURCE_DIR}/Data/images")

# --- Collect sources/headers across subfolders ---
# This updates automatically when you add/remove files (requires CMake 3.12+).
file(GLOB_RECURSE SOURCE_FILES CONFIGURE_DEPENDS
    "${SRC_DIR}/*.cpp"
)

file(GLOB_RECURSE HEADER_FILES CONFIGURE_DEPENDS
    "${INC_DIR}/*.h"
)

add_executable(TemplateSFML
    ${SOURCE_FILES}
    ${HEADER_FILES}
)

target_include_directories(TemplateSFML PRIVATE "${INC_DIR}")

target_link_libraries(TemplateSFML PRIVATE
    sfml-graphics sfml-window sfml-system sfml-network sfml-audio
)

# --- Copy assets next to the executable after build ---
add_custom_command(TARGET TemplateSFML POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
            "${ASSETS_DIR}"
            "$<TARGET_FILE_DIR:TemplateSFML>/assets"
    COMMENT "Copying assets to output directory"
)

# --- Windows: copy OpenAL runtime next to exe (SFML audio dependency) ---
if(WIN32)
    add_custom_command(TARGET TemplateSFML POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
            "${SFML_SOURCE_DIR}/extlibs/bin/$<IF:$<EQUAL:${CMAKE_SIZEOF_VOID_P},8>,x64,x86>/openal32.dll"
            "$<TARGET_FILE_DIR:TemplateSFML>/openal32.dll"
        COMMENT "Copying openal32.dll to output directory"
    )
endif()

file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/Data/ DESTINATION ${CMAKE_CURRENT_BINARY_DIR}/Data/)
