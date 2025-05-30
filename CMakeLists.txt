cmake_minimum_required(VERSION 3.20)

set(WARNINGS_AS_ERRORS_FOR_FOLDER_WATCHER OFF CACHE BOOL "ON iff you want to treat warnings as errors")

add_library(folder_watcher)
add_library(folder_watcher::folder_watcher ALIAS folder_watcher)
target_compile_features(folder_watcher PUBLIC cxx_std_20)

# ---Add source files---
if(WARNINGS_AS_ERRORS_FOR_FOLDER_WATCHER)
    target_include_directories(folder_watcher PUBLIC include)
else()
    target_include_directories(folder_watcher SYSTEM PUBLIC include)
endif()

file(GLOB_RECURSE SRC_FILES CONFIGURE_DEPENDS src/*.cpp)
target_sources(folder_watcher PRIVATE ${SRC_FILES})

# ---Set warning level---
if(MSVC)
    target_compile_options(folder_watcher PRIVATE /W4)
else()
    target_compile_options(folder_watcher PRIVATE -Wall -Wextra -Wpedantic -pedantic-errors -Wconversion -Wsign-conversion)
endif()

# ---Maybe enable warnings as errors---
if(WARNINGS_AS_ERRORS_FOR_FOLDER_WATCHER)
    if(MSVC)
        target_compile_options(folder_watcher PRIVATE /WX)
    else()
        target_compile_options(folder_watcher PRIVATE -Werror)
    endif()
endif()
