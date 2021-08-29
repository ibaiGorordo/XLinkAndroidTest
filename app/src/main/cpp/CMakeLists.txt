# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.10.2)

# Declares and names the project.

project("xlink_android_test")

######         libusb         ######
set(LIBUSB_DIR ${CMAKE_SOURCE_DIR}/../include/libusb)

add_library(usb-1.0 SHARED IMPORTED )
set_target_properties(usb-1.0
        PROPERTIES IMPORTED_LOCATION
        ${CMAKE_SOURCE_DIR}/../lib/libusb/${ANDROID_ABI}/libusb-1.0.so)

include_directories(${LIBUSB_DIR}
        ${LIBUSB_DIR}/android
        ${LIBUSB_DIR}/libusb
        ${LIBUSB_DIR}/libusb/os
        )

######         XLink         ######

#add_subdirectory(XLink)

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

add_library( # Sets the name of the library.
        xlink_android_test

        # Sets the library as a shared library.
        SHARED

        # Provides a relative path to your source file(s).
        native-lib.cpp)

# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

find_library( # Sets the name of the path variable.
        log-lib

        # Specifies the name of the NDK library that
        # you want CMake to locate.
        log)

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

target_link_libraries( # Specifies the target library.
        xlink_android_test

#        XLink

        usb-1.0

        # Links the target library to the log library
        # included in the NDK.
        ${log-lib})