cmake_minimum_required(VERSION 3.7)

PROJECT("openssl")

INCLUDE(ExternalProject)

# Python is used for the patch tool.
FIND_PACKAGE(PythonInterp 2.7 REQUIRED)

#----------------------------------------------------------------------------
#
# Build the project.
#
FIND_PACKAGE(net.zlib-zlib REQUIRED CONFIG)
GET_TARGET_PROPERTY(ZLIB_LIBRARY net.zlib-zlib::zlibstatic LOCATION)
SET(ZLIB_INCLUDE_DIR net.zlib-zlib_INCLUDE_DIR)

IF(${CMAKE_CROSSCOMPILING})
	SET(CROSS_COMPILE_OPTION "--cross-compile-prefix=${CONFIGURE_HOST}")
ELSE(${CMAKE_CROSSCOMPILING})
	SET(CROSS_COMPILE_OPTION "")
ENDIF(${CMAKE_CROSSCOMPILING})

ExternalProject_Add(TARGET_openssl
                    PREFIX ${CMAKE_CURRENT_BINARY_DIR}/openssl
                    URL ${CMAKE_CURRENT_SOURCE_DIR}/openssl-1.1.1g.tar.gz
                    URL_HASH SHA1=b213a293f2127ec3e323fb3cfc0c9807664fd997
                    CONFIGURE_COMMAND ${CMAKE_CURRENT_BINARY_DIR}/openssl/src/TARGET_openssl/config --prefix=${CMAKE_CURRENT_BINARY_DIR}/install ${CROSS_COMPILE_OPTION} --with-zlib-include=${ZLIB_INCLUDE_DIR} --with-zlib-lib=${ZLIB_LIBRARY} no-shared
                    BUILD_COMMAND make
                    INSTALL_COMMAND make install
                    TEST_AFTER_INSTALL 1
                    TEST_COMMAND ${CMAKE_COMMAND} -E copy_directory ${LIBGPGERROR_INCLUDE_DIR} ${CMAKE_CURRENT_BINARY_DIR}/install/include
)
