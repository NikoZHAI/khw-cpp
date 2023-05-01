cmake_minimum_required(VERSION 3.8)
project(khw-cpp
        VERSION 0.1.0
        DESCRIPTION "KVM Hello World with CPP"
        HOMEPAGE_URL "https://github.com/NikoZHAI/khw-cpp"
        LANGUAGES C CXX)

# options

# directories
set(src_dir ${CMAKE_SOURCE_DIR}/src)
set(include_dir ${CMAKE_SOURCE_DIR}/include)
set(external_dir ${CMAKE_SOURCE_DIR}/external)
set(doc_dir ${CMAKE_SOURCE_DIR}/docs)
set(cmake_module_dir ${CMAKE_SOURCE_DIR}/cmake)

# append local cmake modules
set(CMAKE_MODULE_PATH ${cmake_module_dir} ${CMAKE_MODULE_PATH})

# Check for arch
if(NOT (CMAKE_SIZEOF_VOID_P EQUAL 8))
    message(FATAL_ERROR "32-bit builds are not supported")
endif()

# Set default build type
if (NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    set(CMAKE_BUILD_TYPE "Debug")
endif()

# language standards
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS ON)

# find libthread if neccessary
find_package(Threads REQUIRED)

# globbing
file(GLOB driver "main.cpp")
file(GLOB_RECURSE src_c ${src_dir}/*.c)
file(GLOB_RECURSE src_cpp ${src_dir}/*.cpp ${src_dir}/*.cxx ${src_dir}/*.cc)
file(GLOB_RECURSE headers ${include_dir}/*.h ${include_dir}/*.hpp ${src_dir}/*.h ${src_dir}/*.hpp)

# compile driver
set(target "${PROJECT_NAME}")
add_executable(${target} ${driver} ${src_c} ${src_cpp} ${headers})
set_target_properties(${target}
    PROPERTIES LINKER_LANGUAGE CXX
)

# include directories
target_include_directories(${target} PUBLIC
    ${src_dir}
    ${include_dir}
    ${external_dir}
)

# linking
target_link_libraries(${target} PUBLIC
    ${PLATFORM_LIBRARIES}
    ${CMAKE_THREAD_LIBS_INIT}
)
