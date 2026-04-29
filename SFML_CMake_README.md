# C++ SFML CMake Starter

A clean and simple **CMake + SFML starter template** for C++ projects, designed for fast setup, scalability, and minimal maintenance.

This template automatically:
- Fetches SFML 2.6.1
- Detects source and header files (including subdirectories)
- Copies assets into your build output
- Produces a ready-to-run executable

## Features

- CMake 3.16+
- Automatic SFML download via FetchContent
- Recursive file discovery (src/ and include/)
- Flexible asset system (Data/ with subfolders supported)
- Cross-platform (Windows, Linux, macOS)
- Windows audio fix (openal32.dll copy)

## Project Structure

YourProject/
├── CMakeLists.txt
├── README.md
├── include/
│   └── ... headers (can be nested) ...
├── src/
│   └── ... source files (can be nested) ...
└── Data/
    ├── images/
    ├── fonts/
    ├── audio/
    └── ... anything you want ...

## Folder Details

### src/ and include/

You can freely organise your code into subdirectories.

Example:

src/
├── core/
│   └── Game.cpp
├── rendering/
│   └── Renderer.cpp

include/
├── core/
│   └── Game.hpp
├── rendering/
│   └── Renderer.hpp

- All .cpp, .h, and .hpp files are automatically discovered recursively
- You do NOT need to edit CMake when adding new files

### Data/ (Assets)

This is your main asset folder.

You can use any subdirectory structure you want:

Data/
├── images/
├── fonts/
├── audio/
├── shaders/

The entire Data/ folder is copied into your build directory and subdirectories are preserved.

So these will all work:
- ${PROJECT_SOURCE_DIR}/Data/images
- ${PROJECT_SOURCE_DIR}/Data/fonts
- ${PROJECT_SOURCE_DIR}/Data/audio

## Important Note About Assets

The template currently:
1. Copies Data/images → assets/ next to the executable
2. Copies the entire Data/ folder → build directory

You can change this in CMake:

set(ASSETS_DIR "${PROJECT_SOURCE_DIR}/Data")

## Requirements

- CMake 3.16+
- C++ Compiler (MSVC / GCC / Clang)
- Git installed

## Getting Started

1. Clone:
git clone <your-repo-url>

2. Configure:
cmake -S . -B build

3. Build:
cmake --build build

4. Run:
build/bin/

## How It Works

- SFML is fetched using FetchContent
- Source/header files are automatically discovered
- Assets are copied after build
- Windows automatically gets openal32.dll

## Customisation

Rename your project in CMakeLists.txt:

project(TemplateSFML)

Change asset directory if needed:

set(ASSETS_DIR "${PROJECT_SOURCE_DIR}/Data")

## Troubleshooting

SFML not downloading:
→ Ensure Git is installed

Assets not loading:
→ Check paths and build success

New files not detected:
→ Re-run CMake:
cmake -S . -B build

## License

MIT

## Credits

- SFML
- CMake
- Marlowe Baker
- University of The West of England, Bristol
