cmake_minimum_required(VERSION 3.1)
project(SofaHighOrderFEM VERSION 1.0)

set(SOFAHIGHORDERFEM_MAJOR_VERSION 0)
set(SOFAHIGHORDERFEM_MINOR_VERSION 2)
set(SOFAHIGHORDERFEM_VERSION ${SOFAHIGHORDERFEM_MAJOR_VERSION}.${SOFAHIGHORDERFEM_MINOR_VERSION})

set(HEADER_FILES
	HighOrderMeshMatrixMass.h
	HighOrderMeshMatrixMass.inl
    	initHighOrderFEM.h
	HighOrderTetrahedralCorotationalFEMForceField.h 
    	HighOrderTetrahedralCorotationalFEMForceField.inl
	HighOrderTriangularCorotationalFEMForceField.h 
    	HighOrderTriangularCorotationalFEMForceField.inl
	HighOrderTetrahedralDiffusionForceField.h 
    	HighOrderTetrahedralDiffusionForceField.inl
	HighOrderTriangularDiffusionForceField.h 
    	HighOrderTriangularDiffusionForceField.inl
	)

set(SOURCE_FILES
	HighOrderMeshMatrixMass.cpp
	initHighOrderFEM.cpp
    	HighOrderTetrahedralCorotationalFEMForceField.cpp 
    	HighOrderTriangularCorotationalFEMForceField.cpp 
	HighOrderTetrahedralDiffusionForceField.cpp
	HighOrderTriangularDiffusionForceField.cpp
	)

find_package(SofaHighOrderTopology)

add_library(${PROJECT_NAME} SHARED ${HEADER_FILES} ${SOURCE_FILES} )


set(README_FILE "SofaHighOrderFEM.txt" )
set(SOFAHIGHORDERFEM_INCLUDE_DIRS ${CMAKE_CURRENT_SOURCE_DIR})

set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_FLAGS "-DSOFA_BUILD_SOFAHIGHORDERFEM")
target_link_libraries(${PROJECT_NAME} SofaHighOrderTopology SofaBase SofaBaseTopology SofaUserInteraction SofaMiscForceField Sofa.GL)

target_include_directories(${PROJECT_NAME} PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>")
target_include_directories(${PROJECT_NAME} PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>")
target_include_directories(${PROJECT_NAME} PUBLIC "$<INSTALL_INTERFACE:include/${PROJECT_NAME}>")


## Install rules for the library and headers; CMake package configurations files
sofa_create_package_with_targets(
    PACKAGE_NAME ${PROJECT_NAME}
    PACKAGE_VERSION ${PROJECT_VERSION}
    TARGETS ${PROJECT_NAME} AUTO_SET_TARGET_PROPERTIES
    INCLUDE_SOURCE_DIR ${PROJECT_NAME}
    INCLUDE_INSTALL_DIR ${PROJECT_NAME}
    RELOCATABLE "plugins"
    )

#
# Organize projects into sofa-asclepios Visual Studio folders
#
SET_PROPERTY(GLOBAL PROPERTY USE_FOLDERS ON)
SET_PROPERTY(TARGET ${PROJECT_NAME}	PROPERTY FOLDER "SofaHighOrderFEM")
