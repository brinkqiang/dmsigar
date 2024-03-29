
cmake_minimum_required(VERSION 3.2)

PROJECT(sigar)

LIST(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
INCLUDE(cmake/ModuleImport.cmake)
INCLUDE(cmake/ModuleCompileOptions.cmake)
ModuleSetCompileOptions()

INCLUDE(CheckIncludeFiles)
INCLUDE(CheckFunctionExists)
INCLUDE(CheckTypeSize)
INCLUDE(CTest)
INCLUDE(InstallRequiredSystemLibraries)

ENABLE_TESTING()

SET(CPACK_PACKAGE_VERSION_MAJOR "1")
SET(CPACK_PACKAGE_VERSION_MINOR "6")
SET(CPACK_PACKAGE_VERSION_PATCH "2")
SET(CPACK_PACKAGE_VENDOR "Hyperic")

## SET(CPACK_*) before the INCLUDE(CPack)
INCLUDE(CPack)

IF(WIN32) 
	## make sure we only use the smallest set of 
	## headers on win32. Otherwise we get clashes 
	## between winsock2.h and winsock.h
	ADD_DEFINITIONS(-DWIN32_LEAN_AND_MEAN)

	# force the correct version for the redist manifest
	ADD_DEFINITIONS(-D_BIND_TO_CURRENT_MFC_VERSION=1 -D_BIND_TO_CURRENT_CRT_VERSION=1)
	# turn off security warnings for system calls
	ADD_DEFINITIONS(-D_CRT_SECURE_NO_WARNINGS)
	ADD_DEFINITIONS(-D_XKEYCHECK_H)
ELSE(WIN32)
	ADD_COMPILE_OPTIONS(-fgnu89-inline)
ENDIF(WIN32)

ADD_SUBDIRECTORY(src build-src)
ADD_SUBDIRECTORY(tests build-tests)
ADD_SUBDIRECTORY(include build-include)

