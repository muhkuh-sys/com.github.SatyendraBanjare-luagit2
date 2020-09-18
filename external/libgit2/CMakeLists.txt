cmake_minimum_required(VERSION 3.2.3)

PROJECT("libssh2")

INCLUDE(ExternalProject)

# Python is used for some test scripts.
FIND_PACKAGE(PythonInterp 2.7 REQUIRED)

#----------------------------------------------------------------------------
#
# Build the project.
#
SET(PRJ_CMAKE_ARGS "")

# Do not build shared libraries.
LIST(APPEND PRJ_CMAKE_ARGS "-DBUILD_SHARED_LIBS=OFF")

# Do not build the test suite.
LIST(APPEND PRJ_CMAKE_ARGS "-DBUILD_CLAR=OFF")

# Set the search path for libssh2.
LIST(APPEND PRJ_CMAKE_ARGS "-DUSE_SSH=ON")
LIST(APPEND PRJ_CMAKE_ARGS "-DCMAKE_PREFIX_PATH=${CMAKE_BINARY_DIR}/libssh2/install/lib/cmake/libssh2")
#MESSAGE("-DCMAKE_PREFIX_PATH=${CMAKE_BINARY_DIR}/libssh2/install/lib/cmake/libssh2")

# Use the bundled zlib.
LIST(APPEND PRJ_CMAKE_ARGS "-DUSE_BUNDLED_ZLIB=ON")

# Set the location of 
LIST(APPEND PRJ_CMAKE_ARGS "-DLIBGCRYPT_LIBRARY=${CMAKE_BINARY_DIR}/libgcrypt/install/lib/libgcrypt.a")
LIST(APPEND PRJ_CMAKE_ARGS "-DLIBGPG_ERROR_LIBRARY=${CMAKE_BINARY_DIR}/libgpg-error/install/lib/libgpg-error.a")

LIST(APPEND PRJ_CMAKE_ARGS "-DHTTPS_BACKEND=OpenSSL")
LIST(APPEND PRJ_CMAKE_ARGS "-DOPENSSL_FOUND=1")
LIST(APPEND PRJ_CMAKE_ARGS "-DOPENSSL_INCLUDE_DIR=${CMAKE_BINARY_DIR}/openssl/install/include")
LIST(APPEND PRJ_CMAKE_ARGS "-DOPENSSL_CRYPTO_LIBRARY=${CMAKE_BINARY_DIR}/openssl/install/lib/libcrypto.a")
LIST(APPEND PRJ_CMAKE_ARGS "-DOPENSSL_SSL_LIBRARY=${CMAKE_BINARY_DIR}/openssl/install/lib/libssl.a")

LIST(APPEND PRJ_CMAKE_ARGS "-DREGEX_BACKEND=builtin")

ExternalProject_Add(TARGET_libgit2
                    PREFIX ${CMAKE_CURRENT_BINARY_DIR}/libgit2
                    URL ${CMAKE_CURRENT_SOURCE_DIR}/libgit2-1.0.1.tar.gz
                    URL_HASH SHA1=033e0f51b2dca95d0ea857487f914e0e845f282c
                    PATCH_COMMAND "${PYTHON_EXECUTABLE}" ${CMAKE_HOME_DIRECTORY}/../cmake/tools/apply_patches.py --working-folder ${CMAKE_CURRENT_BINARY_DIR}/libgit2/src/TARGET_libgit2 --patch-folder ${CMAKE_CURRENT_SOURCE_DIR}/patches --strip 1
                    CMAKE_ARGS ${PRJ_CMAKE_ARGS}
                    INSTALL_COMMAND make install DESTDIR=${CMAKE_CURRENT_BINARY_DIR}/install
)
