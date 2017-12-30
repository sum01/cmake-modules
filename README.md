# DEPRECIATED

Use [FindBerkeleyDB](https://github.com/sum01/FindBerkeleyDB) or [install_and_export](https://github.com/sum01/install_and_export) instead. I separated the modules for easy `git submodule` support.

---

These are just some [Cmake](https://cmake.org/) modules I made, which I figured someone could benefit from.

# Overview of modules

| Module                                                       | Info                                                                                                                                                                                                                    | Usage                                                                                                |
| :----------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------- |
| [FindBerkeleyDB](cmake/modules/FindBerkeleyDB.cmake)         | Finds [Berkeley DB](http://www.oracle.com/technetwork/database/database-technologies/berkeleydb/overview/index.html), and outputs the paths of the include & libs folders. Note: Only tested on v5 & v6 of Berkeley DB. | `find_package(BerkeleyDB "5.23.0")` which outputs `BERKELEYDB_INCLUDE_DIRS` & `BERKELEYDB_LIBRARIES` |
| [install_and_export](cmake/modules/install_and_export.cmake) | Defines a simple macro for easy installation & exporting of executables/libraries.                                                                                                                                      | `install_and_export(exe_name_here)`                                                                  |

## Usage

To use a module, copy it into a `cmake/modules` folder in your project, then use `list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/modules")` in your main `CmakeLists.txt`.

Afterwards, you can `include(module_name_here)`, or `find_package(PackageName)` for a FindPackage module, to use their functionality.

**Example CmakeLists.txt**

```cmake
# ~~ Example usage in CmakeLists.txt ~~
CMAKE_MINIMUM_REQUIRED(VERSION 3.0 FATAL_ERROR)
project(Example VERSION "1.2.3" LANGUAGES CXX)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/modules")

add_executable(my_exe main.cpp)

# ~~ Example of FindBerkeleyDB usage ~~
find_package(BerkeleyDB "5.23.0" REQUIRED)
target_link_libraries(my_exe PRIVATE ${BERKELEYDB_LIBRARIES})
target_include_directories(my_exe
  PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  PRIVATE ${BERKELEYDB_INCLUDE_DIRS}
)

# ~~ Example of install_and_export usage ~~
include(install_and_export)
# It's necessary when using install_and_export() to use generator expressions on includes
target_include_directories(my_exe
  PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
)
install_and_export(my_exe)
```
