cmake_minimum_required (VERSION 3.2)
project (OpenFace VERSION 1.0.0)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin/)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/modules/")

set(CMAKE_CONFIG_DIR etc/OpenFace)
set(CONFIG_DIR "${CMAKE_INSTALL_PREFIX}/${CMAKE_CONFIG_DIR}")
add_definitions(-DCONFIG_DIR="${CONFIG_DIR}")

# make sure we'll use OpenBLAS only: there's a header file naming difference between different
# implementations; so OpenFace wants cblas.h;
set(BLA_VENDOR "OpenBLAS")
find_package( BLAS )

if ( BLAS_LIBRARIES STREQUAL "FALSE")
	unset(BLAS_LIBRARIES)
endif()

if ( ${BLAS_FOUND} )
    MESSAGE("BLAS information:")
    MESSAGE("  BLAS Vendor: ${BLA_VENDOR}")
    MESSAGE("  BLAS_LIBRARIES: ${BLAS_LIBRARIES}")
    MESSAGE("  BLAS_ROOT_DIR: ${BLAS_ROOT_DIR}")
else()
    message(WARNING "BLAS not found in the system. Using the one vended with OpenFace.")
    include_directories(lib/3rdParty/OpenBLAS/include)
endif()

find_package( OpenCV 3.3 REQUIRED COMPONENTS core imgproc calib3d highgui objdetect)
MESSAGE("OpenCV information:") 
MESSAGE("  OpenCV_INCLUDE_DIRS: ${OpenCV_INCLUDE_DIRS}") 
MESSAGE("  OpenCV_LIBRARIES: ${OpenCV_LIBRARIES}") 
MESSAGE("  OpenCV_LIBRARY_DIRS: ${OpenCV_LINK_DIRECTORIES}") 

find_package( Boost 1.59.0 REQUIRED COMPONENTS filesystem system)
MESSAGE("Boost information:") 
MESSAGE("  Boost_VERSION: ${Boost_VERSION}")
MESSAGE("  Boost_INCLUDE_DIRS: ${Boost_INCLUDE_DIRS}") 
MESSAGE("  Boost_LIBRARIES: ${Boost_LIBRARIES}") 
MESSAGE("  Boost_LIBRARY_DIRS: ${Boost_LIBRARY_DIRS}") 

find_package( TBB CONFIG )
if (NOT ${TBB_FOUND})
    find_package( TBB REQUIRED )
endif()

MESSAGE("TBB information:")
MESSAGE("  TBB_VERSION: ${TBB_VERSION}")
MESSAGE("  TBB_INCLUDE_DIRS: ${TBB_INCLUDE_DIRS}")
MESSAGE("  TBB_LIBRARIES: ${TBB_LIBRARIES}")

# Move LandmarkDetector model
file(GLOB files "lib/local/LandmarkDetector/model/*.txt")
foreach(file ${files})
	if (MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Debug/model)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Release/model)
	else(MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/model)
	endif(MSVC)

	install(FILES ${file} DESTINATION ${CMAKE_CONFIG_DIR}/model)
endforeach()

# Move the hierarchical LandmarkDetector models
file(GLOB files "lib/local/LandmarkDetector/model/model*")
foreach(file ${files})
	if (MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Debug/model)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Release/model)
	else(MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/model)
	endif(MSVC)

	install(DIRECTORY ${file} DESTINATION ${CMAKE_CONFIG_DIR}/model)
endforeach()

file(GLOB files "lib/local/LandmarkDetector/model/detection_validation/*.txt")
foreach(file ${files})
	if (MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Debug/model/detection_validation)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Release/model/detection_validation)
	else(MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/model/detection_validation)
	endif(MSVC)

	install(FILES ${file} DESTINATION ${CMAKE_CONFIG_DIR}/model/detection_validation)
endforeach()

file(GLOB files "lib/local/LandmarkDetector/model/patch_experts/*.txt")
foreach(file ${files})
	if (MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Debug/model/patch_experts)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Release/model/patch_experts)
	else(MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/model/patch_experts)
	endif(MSVC)

	install(FILES ${file} DESTINATION ${CMAKE_CONFIG_DIR}/model/patch_experts)
endforeach()

file(GLOB files "lib/local/LandmarkDetector/model/patch_experts/*.dat")
foreach(file ${files})
	if (MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Debug/model/patch_experts)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Release/model/patch_experts)
	else(MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/model/patch_experts)
	endif(MSVC)

	install(FILES ${file} DESTINATION ${CMAKE_CONFIG_DIR}/model/patch_experts)
endforeach()

file(GLOB files "lib/local/LandmarkDetector/model/pdms/*.txt")
foreach(file ${files})
	if (MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Debug/model/pdms)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Release/model/pdms)
	else(MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/model/pdms)
	endif(MSVC)

	install(FILES ${file} DESTINATION ${CMAKE_CONFIG_DIR}/model/pdms)
endforeach()

# Move OpenCV classifiers
file(GLOB files "lib/3rdParty/OpenCV3.4/classifiers/*.xml")
foreach(file ${files})
	if (MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Debug/classifiers)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Release/classifiers)
	else(MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/classifiers)
	endif(MSVC)

	install(FILES ${file} DESTINATION ${CMAKE_CONFIG_DIR}/classifiers)
endforeach()

# Move AU prediction modules
file(GLOB files "lib/local/FaceAnalyser/AU_predictors/*.txt")
foreach(file ${files})
	if (MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Debug/AU_predictors)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Release/AU_predictors)
	else(MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/AU_predictors)
	endif(MSVC)

	install(FILES ${file} DESTINATION ${CMAKE_CONFIG_DIR}/AU_predictors)
endforeach()

# Move AU prediction modules
file(GLOB files "lib/local/FaceAnalyser/AU_predictors/svr*")
foreach(file ${files})
	if (MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Debug/AU_predictors)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Release/AU_predictors)
	else(MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/AU_predictors)
	endif(MSVC)

	install(DIRECTORY ${file} DESTINATION ${CMAKE_CONFIG_DIR}/AU_predictors)
endforeach()

# Move AU prediction modules
file(GLOB files "lib/local/FaceAnalyser/AU_predictors/svm*")
foreach(file ${files})
	if (MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Debug/AU_predictors)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Release/AU_predictors)
	else(MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/AU_predictors)
	endif(MSVC)

	install(DIRECTORY ${file} DESTINATION ${CMAKE_CONFIG_DIR}/AU_predictors)
endforeach()

if (${CMAKE_CXX_COMPILER_ID} STREQUAL "GNU")
    execute_process(COMMAND ${CMAKE_CXX_COMPILER} -dumpversion OUTPUT_VARIABLE GCC_VERSION)
    if (GCC_VERSION VERSION_LESS 4.7)
        set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x -msse -msse2 -msse3")
    else ()
        set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -msse -msse2 -msse3")
    endif ()
else ()
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -msse -msse2 -msse3")
endif ()

# Boost
if(WIN32)
	include_directories(lib/3rdParty/boost)
	include_directories(lib/3rdParty/boost/boost)
	link_directories( ${PROJECT_SOURCE_DIR}/lib/3rdParty/boost/lib )
else()
	INCLUDE_DIRECTORIES(${BOOST_INCLUDE_DIR})
endif()

# dlib
find_package(dlib 18.18)

if(${dlib_FOUND})
    message("dlib information:")
    message("  dlib version: ${dlib_VERSION}")
    message("  dlib_INCLUDE_DIRS: ${dlib_INCLUDE_DIRS}")

    include_directories(${dlib_INCLUDE_DIRS})
else()
    message(WARNING "dlib not found in the system. Using the one vended with OpenFace.")
    include_directories(lib/3rdParty/dlib/include)

    # dlib library
    add_subdirectory(lib/3rdParty/dlib)
endif()

# suppress auto_ptr deprecation warnings
if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang" OR "${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
    add_compile_options("-Wno-deprecated-declarations")
endif()

# LandmarkDetector library
add_subdirectory(lib/local/LandmarkDetector)
# Facial Expression analysis library
add_subdirectory(lib/local/FaceAnalyser)
# Gaze library
add_subdirectory(lib/local/GazeAnalyser)
# Utilities library
add_subdirectory(lib/local/Utilities)

# export libraries for reuse
include(CMakePackageConfigHelpers)

set(LIB_INSTALL_DIR lib)
set(CONFIG_DEST_DIR ${LIB_INSTALL_DIR}/cmake/OpenFace/)
set(OpenFace_LIBRARIES OpenFace::GazeAnalyser OpenFace::FaceAnalyser OpenFace::LandmarkDetector OpenFace::Utilities)

# export targets [build tree]
export(EXPORT OpenFaceTargets
    NAMESPACE OpenFace::
    FILE "${CMAKE_CURRENT_BINARY_DIR}/${CONFIG_DEST_DIR}/OpenFaceTargets.cmake")

# write package version file
write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/${CONFIG_DEST_DIR}/OpenFaceConfigVersion.cmake"
    COMPATIBILITY AnyNewerVersion)

# define [build tree] bindir relative include dir
foreach(lib ${OpenFace_LIBRARIES})
  if(TARGET ${lib})
    get_target_property(libname ${lib} "NAME")
    file(RELATIVE_PATH rel_incdir ${CMAKE_CURRENT_BINARY_DIR} "${CMAKE_CURRENT_SOURCE_DIR}/lib/local/${libname}/include")
    list(APPEND OPENFACE_INCLUDE_DIRS ${rel_incdir})
  endif()
endforeach()
list(REMOVE_DUPLICATES OPENFACE_INCLUDE_DIRS)

# write package config file from template [build tree]
# all PATH_VARS should be relative to a ${CMAKE_CURRENT_BINARY_DIR}
# as it's the "prefix" of our non installed package in the build tree
configure_package_config_file(cmake/OpenFaceConfig.cmake.in
    "${CMAKE_CURRENT_BINARY_DIR}/${CONFIG_DEST_DIR}/OpenFaceConfig.cmake"
    INSTALL_DESTINATION ${CONFIG_DEST_DIR}
    PATH_VARS OPENFACE_INCLUDE_DIRS)

# store current build dir in the CMake package registry
export(PACKAGE OpenFace)

# test if this file is a top list list file
# thus we're building an OpenFace as a standalone
# project; otherwise OpenFace is being built as a
# part or larger tree
if(CMAKE_CURRENT_SOURCE_DIR STREQUAL "${CMAKE_SOURCE_DIR}")
    # for a standalone builds - allow installing package configs;
    message(STATUS "Standalone mode detected")

    # install exported targets [install tree]
    install(EXPORT OpenFaceTargets
        FILE OpenFaceTargets.cmake
        NAMESPACE OpenFace::
        DESTINATION ${CONFIG_DEST_DIR})

    # redefine [install tree] prefix relative include dir
    set(OPENFACE_INCLUDE_DIRS "include/OpenFace")

    # write package config file from template [install tree]
    configure_package_config_file(cmake/OpenFaceConfig.cmake.in
        "${CMAKE_CURRENT_BINARY_DIR}/OpenFace/OpenFaceConfig.cmake"
        INSTALL_DESTINATION ${CONFIG_DEST_DIR}
        PATH_VARS OPENFACE_INCLUDE_DIRS)

    # install package configs
    install(FILES
        "${CMAKE_CURRENT_BINARY_DIR}/OpenFace/OpenFaceConfig.cmake"
        "${CMAKE_CURRENT_BINARY_DIR}/${CONFIG_DEST_DIR}/OpenFaceConfigVersion.cmake"
        DESTINATION ${CONFIG_DEST_DIR})
endif()

# executables
add_subdirectory(exe/FaceLandmarkImg)
add_subdirectory(exe/FaceLandmarkVid)
add_subdirectory(exe/FaceLandmarkVidMulti)
add_subdirectory(exe/FeatureExtraction)
