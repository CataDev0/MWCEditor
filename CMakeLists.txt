cmake_minimum_required(VERSION 3.10)

project(MWCEditor VERSION 2.2 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Enable Windows Resource compiling
if(WIN32 OR MINGW)
    enable_language(RC)
    add_compile_definitions(UNICODE _UNICODE)
endif()

# Source files
set(SOURCES
    MWCeditor/debug_dump.cpp
    MWCeditor/dlgprocs.cpp
    MWCeditor/externs.cpp
    MWCeditor/grouping.cpp
    MWCeditor/main.cpp
    MWCeditor/map.cpp
    MWCeditor/map_dlgprocs.cpp
    MWCeditor/resize.cpp
    MWCeditor/utils.cpp
    MWCeditor/variable.cpp
    MWCeditor/MWCeditor.rc
)

# Create the Windows executable (GUI app)
add_executable(MWCEditor WIN32 ${SOURCES})

# Add the include directory so headers are found
target_include_directories(MWCEditor PRIVATE MWCeditor)

if(MINGW)
    # Target Windows statically to avoid requiring runtime DLLs
    target_link_options(MWCEditor PRIVATE "-static")
    
    # Required Windows libraries
    target_link_libraries(MWCEditor PRIVATE
        comctl32
        shlwapi
        version
    )
endif()
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -municode")
