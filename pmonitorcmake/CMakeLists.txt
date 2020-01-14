# From https://root.cern.ch/faq/can-i-integrate-root-my-cmake-build

# CMakeLists.txt for event package. It creates a library with dictionary and a main program
cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(pmonitor)

# You need to tell CMake where to find the ROOT installation. This can be done in a number of ways:
#   - ROOT built with classic configure/make use the provided $ROOTSYS/etc/cmake/FindROOT.cmake
#   - ROOT built with CMake. Add in CMAKE_PREFIX_PATH the installation prefix for ROOT
list(APPEND CMAKE_PREFIX_PATH $ENV{ROOTSYS})

#---Locate the ROOT package and defines a number of variables (e.g. ROOT_INCLUDE_DIRS)
find_package(ROOT REQUIRED COMPONENTS Core RIO RooFit RooFitCore RooStats Hist Tree Matrix Physics MathCore Net)

#---Define useful ROOT functions and macros (e.g. ROOT_GENERATE_DICTIONARY)
include(${ROOT_USE_FILE})

include_directories(${CMAKE_SOURCE_DIR} ${ROOT_INCLUDE_DIRS} $ENV{ONLINE_MAIN}/include)
add_definitions(${ROOT_CXX_FLAGS})

#include source and include dir
include_directories(${PROJECT_SOURCE_DIR}/include)
file(GLOB sources ${PROJECT_SOURCE_DIR}/src/*.cc)
file(GLOB headers ${PROJECT_SOURCE_DIR}/include/*.h)


link_directories($ENV{ONLINE_MAIN}/lib)

#---Create a shared library with geneated dictionary
ROOT_GENERATE_DICTIONARY(G__pmonitor ${PROJECT_SOURCE_DIR}/include/pmonitor.h LINKDEF ${PROJECT_SOURCE_DIR}/include/pmonitorLinkDef.h)
add_library(pmonitor SHARED ${sources} ${headers}  G__pmonitor.cxx)
target_link_libraries(pmonitor Event NoRootEvent message)
target_link_libraries(pmonitor ${ROOT_LIBRARIES})

#----------------------------------------------------------------------------
# Install the library to 'bin' directory under CMAKE_INSTALL_PREFIX
#
set_target_properties(pmonitor PROPERTIES PUBLIC_HEADER "${headers}")
install(TARGETS pmonitor 
        LIBRARY DESTINATION lib
        PUBLIC_HEADER DESTINATION include/pmonitor
)
install (FILES ${PROJECT_BINARY_DIR}/libpmonitor_rdict.pcm DESTINATION lib)


