cmake_minimum_required(VERSION 2.8.8)

project(PrismCurl)

if(ISD)
    set(PLATFORM isd)
elseif(APPLE)
    set(PLATFORM osx)
elseif(WIN32)
    set(PLATFORM win32)
else()
    set(PLATFORM linux)
endif()

#
# OpenSSL
#
add_subdirectory(3rdParty/OpenSSL)

#
# curl
#

#Clean the platform install directory every time
set(CLEAN_DIR ${CMAKE_CURRENT_SOURCE_DIR}/install/${PLATFORM})
MESSAGE("DELETE " ${CLEAN_DIR})
file(REMOVE_RECURSE ${CLEAN_DIR})

set(CMAKE_INSTALL_PREFIX ${CMAKE_CURRENT_SOURCE_DIR}/install/${PLATFORM} CACHE INTERNAL "")

set(HAVE_GLIBC_STRERROR_R 0 CACHE INTERNAL "")
set(HAVE_GLIBC_STRERROR_R__TRYRUN_OUTPUT 0 CACHE INTERNAL "")
set(HAVE_POSIX_STRERROR_R 1 CACHE INTERNAL "")
set(HAVE_POSIX_STRERROR_R__TRYRUN_OUTPUT 0 CACHE INTERNAL "")
set(HAVE_POLL_FINE 0 CACHE INTERNAL "")
set(HAVE_POLL_FINE__TRYRUN_OUTPUT 0 CACHE INTERNAL "")

set(BUILD_CURL_EXE 0 CACHE INTERNAL "")
set(BUILD_CURL_TESTS 0 CACHE INTERNAL "")
set(HTTP_ONLY 1 CACHE INTERNAL "")

#if(NOT BUILD_SHARED_LIBS)
    set(CURL_STATICLIB 1 CACHE INTERNAL "")
#endif()

add_subdirectory(src)

if(HAVE_SYS_SOCKET_H)
    # TODO: Fix curbuild.h.cmake upstream so that we don't have to do
    # this.  (curlbuild.h.in doesn't require it.)
    set_property(DIRECTORY  APPEND PROPERTY COMPILE_DEFINITIONS HAVE_SYS_SOCKET_H)
endif()
#
## Make curl_base64.h available.
#configure_file(${CURL_SOURCE_DIR}/lib/curl_base64.h
#    ${CURL_BINARY_DIR}/include/curl COPY_ONLY)
#
set(CURL_LIBRARY libcurl CACHE INTERNAL "")
set(CURL_INCLUDE_DIR ${CURL_BINARY_DIR}/include/curl ${CURL_SOURCE_DIR}/include CACHE INTERNAL "")
#set_target_properties(libcurl PROPERTIES FOLDER "3rdParty2")