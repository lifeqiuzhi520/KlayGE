PROJECT(AndroidNativeAppGlue C)

CMAKE_MINIMUM_REQUIRED(VERSION 2.8.6)

SET(NDK_VERSION 16b)

SET(ANAG_PROJECT_DIR "${CMAKE_CURRENT_SOURCE_DIR}/../..")
SET(KLAYGE_ROOT_DIR "${ANAG_PROJECT_DIR}/../..")

INCLUDE(${KLAYGE_ROOT_DIR}/cmake/Common.cmake)
INCLUDE(${KLAYGE_ROOT_DIR}/cmake/Platform.cmake)
INCLUDE(${KLAYGE_ROOT_DIR}/cmake/Compiler.cmake)
INCLUDE(${KLAYGE_ROOT_DIR}/External/Build/CMake/ExternalCommon.cmake)

SET(ANAG_OUTPUT_DIR "${ANAG_PROJECT_DIR}/lib/${KLAYGE_PLATFORM_NAME}")
SET(REL_PATH "External/Downloads/android_native_app_glue_${NDK_VERSION}.patched.7z")
SET(SEVENZIP_PATH "${KLAYGE_ROOT_DIR}/External/Downloads/${KLAYGE_HOST_PLATFORM_NAME}/7z")
SET(PACKAGE_FILE_ID "b32103ef58937c827b9a1dba3070f49d8c9ec1c0")

DOWNLOAD_PACKAGE("android_native_app_glue" ${REL_PATH} ${PACKAGE_FILE_ID})

FOREACH(flag_var
		CMAKE_C_FLAGS CMAKE_CXX_FLAGS)
	IF(KLAYGE_COMPILER_MSVC)
		STRING(REPLACE "/W4" "/W0" ${flag_var} "${${flag_var}}")
		STRING(REPLACE "/WX" "/WX-" ${flag_var} "${${flag_var}}")
	ELSE()
		STRING(REPLACE "-Wall" "-w" ${flag_var} "${${flag_var}}")
		STRING(REPLACE "-Werror" "-Wno-error" ${flag_var} "${${flag_var}}")
	ENDIF()
ENDFOREACH()

SET(ANAG_SOURCES
	${ANAG_PROJECT_DIR}/android_native_app_glue.c
	${ANAG_PROJECT_DIR}/android_native_app_glue.h
)

ADD_LIBRARY(android_native_app_glue ${ANAG_SOURCES})

SET_TARGET_PROPERTIES(android_native_app_glue PROPERTIES
	ARCHIVE_OUTPUT_DIRECTORY ${ANAG_OUTPUT_DIR}
	ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${ANAG_OUTPUT_DIR}
	ARCHIVE_OUTPUT_DIRECTORY_RELEASE ${ANAG_OUTPUT_DIR}
	ARCHIVE_OUTPUT_DIRECTORY_RELWITHDEBINFO ${ANAG_OUTPUT_DIR}
	ARCHIVE_OUTPUT_DIRECTORY_MINSIZEREL ${ANAG_OUTPUT_DIR}
	PROJECT_LABEL android_native_app_glue
	DEBUG_POSTFIX ${CMAKE_DEBUG_POSTFIX}
	OUTPUT_NAME android_native_app_glue${KLAYGE_OUTPUT_SUFFIX}
)
