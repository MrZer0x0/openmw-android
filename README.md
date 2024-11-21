
## OpenMW Mobile 📲 for Android 
## version by OTF
[Telegram](https://t.me/morrowind24)

(thank you for the constructor and efforts
sandstranger, xyzz, and sisah)

## Build

There are two stages to building OpenMW for Android. The first stage is building the C/C++ libraries. The second stage is building the Java loader.

### Prerequisites

You'll need some standard tools, which are likely already installed (bash, gcc, g++, sha256sum, unzip).

CMake 3.6.0 or newer **is mandatory**, you can download the latest version [here](https://cmake.org/download/) (and put it in your `PATH`) if your distribution comes with an old version.

Additionally, to build the loader, you'll need an installed Android SDK; it's recommended to use Android Studio, which can set it up for you (see step 2).

### Step 1: Build Libraries

Navigate to the `buildscripts` directory and run `./build.sh`. The script will automatically download the Android native toolchain and all dependencies, as well as compile and install them.

### Step 2: Build Java Loader

To get an APK file that you can install, open the `openmw-android` directory in Android Studio and run the project.

Alternatively, if you do not have Android Studio installed or do not wish to use it, run `./gradlew assembleDebug` from the root directory of this repository. The resulting APK, located at `./app/build/outputs/apk/debug/app-debug.apk`, can be transferred to the device and installed.

## Notes for Developers

### Debugging Native Code

You can debug native code using `ndk-gdb`. To use it, after building the libraries and APK and installing the APK, launch the app and leave it at the main menu. Then `cd` to `app/src/main` and run `./gdb.sh [arch]`. The `arch` variable must match the library that your device will use (one of `arm`, `arm64`, `x86_64`, `x86`; default is `arm`).

This also automatically includes gdb for using unstripped libraries, so you will get correct symbols, source code references, etc.

### Running Address Sanitizer

To compile everything with ASAN:


# Clean previous build
./clean.sh
# Build with ASAN enabled and debug symbols
./build.sh --ccache --asan --debug
# Or: ./build.sh --ccache --asan --debug --arch arm64

Then open Android Studio and compile and install the project.

To get symbolized output:

adb logcat | ./tool/asan_symbolize.py --demangle -s ./symbols/armeabi-v7a/
# Or: adb logcat | ./tool/asan_symbolize.py --demangle -s ./symbols/arm64-v8a/

## Thank You

### Source Code

The original Java code is written by sandstranger. The build scripts were originally written by sandstranger, bwhaines and sisah
