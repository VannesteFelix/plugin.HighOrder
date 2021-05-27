cmake_minimum_required(VERSION 3.1)
project(SofaHighOrderTopology VERSION 1.0)

find_package(SofaBase)

set(SOFAHIGHORDERTOPOLOGY_MAJOR_VERSION 0)
set(SOFAHIGHORDERTOPOLOGY_MINOR_VERSION 2)
set(SOFAHIGHORDERTOPOLOGY_VERSION ${SOFAHIGHORDERTOPOLOGY_MAJOR_VERSION}.${SOFAHIGHORDERTOPOLOGY_MINOR_VERSION})

set(HEADER_FILES
	Mesh2HighOrderTopologicalMapping.h
	Bezier2MeshMechanicalMapping.h
    Bezier2MeshMechanicalMapping.inl
	GenerateBezierSphere.h
    GenerateBezierSphere.inl
	GenerateBezierCylinder.h
    GenerateBezierCylinder.inl	
    HighOrder2MeshTopologicalMapping.h
    HighOrderTetra2HighOrderTriangleTopologicalMapping.h
    initHighOrderTopology.h
	HighOrderTetrahedronSetTopologyContainer.h
	HighOrderTriangleSetTopologyContainer.h
	BezierTriangleSetGeometryAlgorithms.h
	BezierTriangleSetGeometryAlgorithms.inl
	HighOrderTetrahedronSetGeometryAlgorithms.h
	HighOrderTetrahedronSetGeometryAlgorithms.inl
	HighOrderTriangleSetGeometryAlgorithms.h
	HighOrderTriangleSetGeometryAlgorithms.inl
	BezierTetrahedronSetGeometryAlgorithms.h
	BezierTetrahedronSetGeometryAlgorithms.inl
	LagrangeTetrahedronSetGeometryAlgorithms.h
	LagrangeTetrahedronSetGeometryAlgorithms.inl
	LagrangeTriangleSetGeometryAlgorithms.h
	LagrangeTriangleSetGeometryAlgorithms.inl
	HighOrderTopologyIndex.h
        # MeshHOMFLoader.h
	HOMFFileExporter.h
	HOMFFileExporter.inl
	)

set(SOURCE_FILES
	GenerateBezierSphere.cpp
	GenerateBezierCylinder.cpp
	Mesh2HighOrderTopologicalMapping.cpp
	Bezier2MeshMechanicalMapping.cpp
    HighOrder2MeshTopologicalMapping.cpp
    HighOrderTetra2HighOrderTriangleTopologicalMapping.cpp
	initHighOrderTopology.cpp
	HighOrderTetrahedronSetTopologyContainer.cpp
	HighOrderTriangleSetTopologyContainer.cpp
	BezierTriangleSetGeometryAlgorithms.cpp
	BezierTetrahedronSetGeometryAlgorithms.cpp
	LagrangeTetrahedronSetGeometryAlgorithms.cpp
	LagrangeTriangleSetGeometryAlgorithms.cpp
        # MeshHOMFLoader.cpp
	HOMFFileExporter.cpp
	)


add_library(${PROJECT_NAME} SHARED ${HEADER_FILES} ${SOURCE_FILES} )


set(README_FILE "SofaHighOrderTopology.txt" )
set(SOFAHIGHORDERTOPOLOGY_INCLUDE_DIRS ${CMAKE_CURRENT_SOURCE_DIR})

set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_FLAGS "-DSOFA_BUILD_SOFAHIGHORDERTOPOLOGY")
target_link_libraries(${PROJECT_NAME} SofaBase SofaBaseTopology SofaUserInteraction SofaMiscForceField)

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


set(SOFAHIGHORDERTOPOLOGY_INCLUDE_DIRS ${CMAKE_SOURCE_DIR}) 

#
# Organize projects into sofa-asclepios Visual Studio folders
#
SET_PROPERTY(GLOBAL PROPERTY USE_FOLDERS ON)
SET_PROPERTY(TARGET ${PROJECT_NAME}	PROPERTY FOLDER "SofaHighOrderTopology")


if(SOFA_BUILD_TESTS)
    find_package(SofaTest QUIET)
    if(SofaTest_FOUND)
        add_subdirectory(SofaHighOrderTopology_test)
    endif()
endif()
