project(baresip)

cmake_minimum_required(VERSION 3.4.1)

add_link_options("LINKER:--build-id=none")

set(distribution_DIR ${CMAKE_SOURCE_DIR}/../../../../libbaresip-android/distribution.video)

add_library(lib_vpx STATIC IMPORTED)
set_target_properties(lib_vpx PROPERTIES IMPORTED_LOCATION
        ${distribution_DIR}/${ANDROID_ABI}/lib/libvpx.a)

add_library(lib_crypto STATIC IMPORTED)
set_target_properties(lib_crypto PROPERTIES IMPORTED_LOCATION
        ${distribution_DIR}/${ANDROID_ABI}/lib/libcrypto.a)

add_library(lib_ssl STATIC IMPORTED)
set_target_properties(lib_ssl PROPERTIES IMPORTED_LOCATION
        ${distribution_DIR}/${ANDROID_ABI}/lib/libssl.a)

add_library(lib_re STATIC IMPORTED)
set_target_properties(lib_re PROPERTIES IMPORTED_LOCATION
        ${distribution_DIR}/${ANDROID_ABI}/lib/libre.a)

add_library(lib_baresip STATIC IMPORTED)
set_target_properties(lib_baresip PROPERTIES IMPORTED_LOCATION
        ${distribution_DIR}/${ANDROID_ABI}/lib/libbaresip.a)

add_library(baresip SHARED baresip.c)

target_include_directories(baresip PRIVATE
        ${distribution_DIR}/${ANDROID_ABI}/include)

add_definitions(-DHAVE_PTHREAD)

target_link_libraries(baresip
        android
        aaudio
        lib_baresip
        lib_re
        lib_ssl
        lib_crypto
        lib_vpx
        z
        log)
