cmake_minimum_required ( VERSION 2.8)
PROJECT ( TubularGeodesicsFijiPlugin )

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG 	./Debug)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE 	./Release)

# Standard:          /DWIN32 /D_WINDOWS /W3 /Zm1000 /EHsc /GR
IF (WIN32)
		SET(CMAKE_CXX_FLAGS "/DWIN32 /D_WINDOWS /W4 /Zi /EHsc /GR- /MP /openmp /bigobj") 
		ADD_DEFINITIONS(-D_OPENMP -DFFTW_DLL -DLIBFFTWF33_EXPORTS) # Visual Studio 2005 and up supports OpenMP
ENDIF (WIN32)

find_package(ITK REQUIRED)
if(ITK_FOUND)
	include(${ITK_USE_FILE})
endif()

FIND_PACKAGE(OpenMP)
if (OPENMP_FOUND)
  SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
  SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}" )
ELSE(OPENMP_FOUND)
  MESSAGE(FATAL_ERROR "Cannot build without OpenMP.")
endif(OPENMP_FOUND)


set ( TubularGeodesicsFijiPlugin_INCLUDE_DIR  
	${CMAKE_CURRENT_SOURCE_DIR}/c++/
	${CMAKE_CURRENT_SOURCE_DIR}/itkCVLab/
	${CMAKE_CURRENT_SOURCE_DIR}/FijiITKInterface/
	C:/Java/jdk1.7.0_07/include/
	C:/Java/jdk1.7.0_07/include/win32/
	${ITK_INCLUDE_DIR}
	C:/Users/fbenmans/Downloads/fftw-3.3.3-build/Release/
)

include_directories( ${TubularGeodesicsFijiPlugin_INCLUDE_DIR})
include_directories( ${TubularGeodesicsFijiPlugin_INCLUDE_DIR})


file(GLOB	TubularGeodesics_SOURCE 	c++/TubularGeodesicsJNIImplementation.cpp
										FijiITKInterface/FijiITKInterface_TubularGeodesics.h)
file(GLOB	OOFTubularityMeasure_SOURCE 	c++/OOFTubularityMeasureJNIImplementation.cpp
											FijiITKInterface/FijiITKInterface_OOFTubularityMeasure.h)


ADD_LIBRARY(TubularGeodesics	 SHARED ${TubularGeodesics_SOURCE})
TARGET_LINK_LIBRARIES(TubularGeodesics ${ITK_LIBRARIES})

ADD_LIBRARY(OOFTubularityMeasure SHARED ${OOFTubularityMeasure_SOURCE})
TARGET_LINK_LIBRARIES(OOFTubularityMeasure ${ITK_LIBRARIES} fftw3)

