cmake_minimum_required(VERSION 2.8)

project(bundle_adjustment)
set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_FLAGS "-O3 -std=c++14")
set( CMAKE_CXX_STANDARD 14)

if (POLICY CMP0072)
    set(OpenGL_GL_PREFERENCE LEGACY)
endif()

Find_Package(g2o REQUIRED)
# Find_Package(Eigen3 REQUIRED)
Find_Package(Ceres REQUIRED)
Find_Package(Sophus REQUIRED)
# Find_Package(CSparse REQUIRED)
Find_Package(fmt REQUIRED)

# SET(G2O_LIBS g2o_csparse_extension g2o_stuff g2o_core cxsparse)
SET(G2O_LIBS g2o::g2o_ceres_ad g2o::stuff g2o::opengl_helper g2o::core
        g2o::g2o_hierarchical_library g2o::g2o_simulator_library
        g2o::types_slam2d g2o::types_slam2d_addons
        g2o::solver_pcg g2o::solver_dense g2o::solver_slam2d_linear
        g2o::solver_structure_only g2o::solver_csparse g2o::csparse_extension g2o::solver_cholmod g2o::solver_eigen
         g2o::csparse_extension g2o::stuff g2o::core
#        cxsparse
        )

# Eigen
include_directories("/usr/include/eigen3")

add_library(bal_common common.cpp)
add_executable(bundle_adjustment_g2o bundle_adjustment_g2o.cpp)
add_executable(bundle_adjustment_ceres bundle_adjustment_ceres.cpp)

target_link_libraries(bundle_adjustment_ceres Ceres::ceres bal_common)
target_link_libraries(bundle_adjustment_g2o ${G2O_LIBS} bal_common ${Sophus_LIBRARIES})


#Fail
#cmake_minimum_required(VERSION 2.8)
#
#project(bundle_adjustment)
#set(CMAKE_BUILD_TYPE "Release")
#set(CMAKE_CXX_STANDARD 14)
#
#LIST(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)
#
#Find_Package(G2O REQUIRED)
#Find_Package(Eigen3 REQUIRED)
#Find_Package(Ceres REQUIRED)
#Find_Package(Sophus REQUIRED)
#Find_Package(CSparse REQUIRED)
#find_package(fmt REQUIRED)
#
#SET(G2O_LIBS g2o_csparse_extension g2o_stuff g2o_core cxsparse)
#
#include_directories(${PROJECT_SOURCE_DIR} ${EIGEN3_INCLUDE_DIR} ${CSPARSE_INCLUDE_DIR})
#
#add_library(bal_common common.cpp)
#add_executable(bundle_adjustment_g2o bundle_adjustment_g2o.cpp)
#add_executable(bundle_adjustment_ceres bundle_adjustment_ceres.cpp)
#
#target_link_libraries(bundle_adjustment_ceres ${CERES_LIBRARIES} bal_common)
#target_link_libraries(bundle_adjustment_g2o ${G2O_LIBS} bal_common ${Sophus_LIBRARIE} fmt::fmt)
