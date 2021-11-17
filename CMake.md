## Basic
- cmake_minimum_required(VERSION 3.10)
- project (project_name)
- add_executable(project_name source_files)
- Directory Path
  - CMAKE_SOURCE_DIR: the root source directory
  - CMAKE_CURRENT_SOURCE_DIR: the current source directory if using sub-projects and directories
  - PORJECT_SOURCE_DIR: the source directory of the current cmake project
  - CMAKE_BINARY_DIR: the root binary / build directory. this is the directory where you ran the cmake command
  - CMAKE_CURRENT_BINARY_DIR: the build directory you are currently in
  - PROJECT_BINARY_DIR: the build directory for the current project

### Executable
- source files: set(SOURCES src/hello.cpp src/main.cpp) == file(GLOB SORUCES "src/*.cpp")
- header files: target_include_directories(target PRIVATE ${PROJECT_SOURCE_DIR}/include) == -I/directory/path/
- static lib: target_link_libraries(project_name PRIVATE lib_name)
- shared lib: target_link_libraries(project_name PRIVATE lib_name/lib::alias_name)
- target_compile_definitions(project_name PRIVATE EX3)
- third_party: find_package(third_lib_name version REQUIRED COMPONENTS filesystem system)
  - find_package(Boost 1.46.1 REQUIRED COMPONENTS filesystem system)
  - Boost: name of library
  - 1.46.1: minimum version of boost to find
  - REQUIRED: to fail if it cannot be found
  - COMPONENTS: the list libraries to find
  - target_link_libraries(project_name PRIVATE Boost::filesystem)
- compile with clang
  - CMAKE_C_COMPILER: the program used to compile c code
  - CMAKE_CXX_COMPILER: the program used to compile c++ code
  - CMAKE_LINKER: the program used to link your library
  - cmake .. -DCMAKE_C_COMPILER=clang-3.6 -DCMAKE_CXX_COMPILER=clang++-3.6
- build with ninja
  - ninja_supported=`cmake --help | grep Ninja`
  - cmake .. -G Ninja && ninja
- CXX-standard
  - set (CMAKE_CXX_STANDARD 11)
- target_compile_features(project_name PUBLIC cxx_auto_type)


### Static Lib
- add_library(lib_name STATIC src/h.cpp)
- target_include_directories(lib_name PUBLIC dir/include)


### Shared Lib
- add_library(lib_name SHARED src/h.cpp)
- add_library(lib::alias_name ALIAS lib_name)
- target_include_directories(lib_name PUBLIC dir/include)


### Install
- Binaries: install (TARGETS project_name DESTINATION bin)
- Library: install (TARGETS project_name LIBRARY DESTINATION lib) # may not work on windows
- Header files: install (DIRECTORY dir/include/ DESTINATION include)
- Config: install (FILES cmake-examples.conf DESTINATION etc)
- the base install location is controlled by the variable CMAKE_INSTALL_PREFIX which can be set using ccmake or by calling cmake with `cmake .. -DCMAKE_INSTALL_PREFIX=/install/location`
- make install
















