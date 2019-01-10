tutorial-2-mixedprocessing
=================================

This application is a sample of OpenCV4Android.

It shows ways of  pre-processing camera preview frames with both Java and C++ calls to OpenCV.

It works with Android Studio 3.0.1

Last included OpenCV version: 3.4.3



Usage
-----

Here is how to use this project to run sample OpenCV code.

* Make sure you have Android SDK up to date, with NDK installed and CMake
* Download latest OpenCV SDK for Android from OpenCV.org and decompress the zip file.
* Clone this project
* In `app/CMakeLists.txt` change line 9 to points to `YOUR_OPENCV_SDK/sdk/native/jni/include`
* Sync gradle
* Run the application


How to import the Elicpse project to Android Studio
----------------------------------------------------

* Make sure you have Android SDK up to date, with NDK installed
* Download latest OpenCV SDK for Android from OpenCV.org and decompress the zip file to 
  e.g. `C:\Android_w\opencv-3.4.3-android-sdk`

* Import project
  * Delete the project properties of tutorial-2-mixedprocessing

      Edit `project.properties`
      ```
        #android.library.reference.1=../../sdk/java
        #target=android-11
      ```

  * New -> Import Project
  * Choose the tutorial-2-mixedprocessing
  * Set the distributionUrl up to fit your SDK

      Edit `gradle-wrapper.properties` to fit your SDK:
      ```
        distributionUrl=https\://services.gradle.org/distributions/gradle-4.1-all.zip

      ```

* Import OpenCV library module
  * New -> Import Module
  * Choose the `YOUR_OPENCV_SDK/sdk/java` folder
  * Unckeck replace jar, unckeck replace lib, unckeck create gradle-style

* Set the OpenCV library module up to fit your SDK

  Edit `app/build.gradle` and `openCVLibrary343/build.gradle` to fit your SDK:

  ```
    compileSdkVersion 26
    defaultConfig {
        minSdkVersion 23
        targetSdkVersion 26
    }
  ```

* Add OpenCV module dependency in your app module

  File -> Project structure -> Module app -> Dependencies tab -> New module dependency -> choose OpenCV library module

* Create `CMakeLists.txt` file at app/ser/main/jni

    ```
    set (OPENCV_SDK_DIR C:\\Android_w\\opencv-3.4.3-android-sdk)
    include_directories(${OPENCV_SDK_DIR}/opencv-android-sdk/sdk/native/jni/include)
    add_library( lib_opencv SHARED IMPORTED )
    set_target_properties(lib_opencv PROPERTIES IMPORTED_LOCATION ${OPENCV_SDK_DIR}/opencv-android-sdk/sdk/native/libs/${ANDROID_ABI}/libopencv_java3.so)

    add_library(mixed_sample SHARED jni_part.cpp)
    find_library(log-lib log)
    target_link_libraries(mixed_sample lib_opencv ${log-lib})
    ```

* Set the app build.gradle
  * Add externalNativeBuild
    ```
        externalNativeBuild {
            cmake {
                path "src/main/jni/CMakeLists.txt"
            }
        }
    ```

* Sync gradle

* About the camera permission
  * Settings > Apps > OCV T2 Mixed Processing > Permissions > turn on Camera

