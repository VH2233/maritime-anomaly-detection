cmake_minimum_required(VERSION 3.20)
project(maritime_anomaly_detection VERSION 0.1.0 LANGUAGES CXX)

# Use modern C++
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Generate compile_commands.json for editor autocompletion
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

add_executable(mda src/main.cpp)
target_include_directories(mda PRIVATE include)

# Strict warnings catch bugs early
target_compile_options(mda PRIVATE -Wall -Wextra -Wpedantic)
