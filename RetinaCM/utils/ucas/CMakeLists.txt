# find source files
file(GLOB ucas_src *.h *.cpp *.hpp)

# create static library from source files (and optionally link with GDCM)
option (WITH_GDCM "Use GDCM to read DICOM images" OFF)
if(WITH_GDCM)
	message(STATUS "Include GDCM ...")
	find_package(GDCM REQUIRED)
	if(NOT GDCM_FOUND)
	    message(FATAL_ERROR "GDCM library not found or not properly installed")
	endif()
	message(STATUS "Include GDCM ... OK!")
	add_definitions(-DWITH_GDCM)
	INCLUDE(${GDCM_USE_FILE})
	add_library(ucasUtils STATIC ${ucas_src})
	target_link_libraries(ucasUtils gdcmMSFF)
else()
	add_library(ucasUtils STATIC ${ucas_src})
endif(WITH_GDCM)