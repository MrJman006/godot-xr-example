This page contains the steps to create a Godot project capable of running on a Meta Quest 2.

## References

- https://www.youtube.com/watch?v=7XWyZblSnZA
- https://docs.godotengine.org/en/stable/tutorials/xr/index.html
- https://docs.godotengine.org/en/stable/tutorials/xr/deploying_to_android.html
- https://docs.godotengine.org/en/stable/tutorials/export/exporting_for_android.html

## Steps

1. Install Godot. Used version 4.2.1.

2. Open the Godot project explorer and create a new project named `Godot XR Example`. Set the `Renderer` to `Compatibility` and `Version Control Metadata` to `Git`.

2. Installed CMake. Used version 3.29.

3. Installed JDK. Used version 17.

4. Installed Android SDK Command Line Tools. Installed to `~/Projects/dependencies/android-sdk/cmdline-tools/latest`.

5. Installed the Android SDK components listed below. The commands used are in the code block.

|SDK Component  |Version |
|:--------------|:-------|
|Platform-Tools |30.0.5+ |
|Build-Tools    |33.0.2+ |
|Platform Image |33+     |
|NDK            |r23c    |

```
ANDROID_SDK_ROOT="${HOME}/Projects/dependencies/android-sdk"
SDKMANAGER="${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin/sdkmanager.bat"
COMPONENT_LIST=()
COMPONENT_LIST+=("platform-tools")
COMPONENT_LIST+=("build-tools;33.0.2")
COMPONENT_LIST+=("platforms;android-33")
COMPONENT_LIST+=("ndk;23.2.8568313")
cmd //c "${SDKMANAGER} ${COMPONENT_LIST[@]}"
```

6. Created a `debug.keystore` file using `JDK keytool`.

```
JDK_ROOT="${HOME}/Projects/dependencies/jdk/v17"
KEYTOOL="${JDK_ROOT}/bin/keytool.exe"
DEBUG_KEYSTORE_FILE="${HOME}/.android/debug.keystore"
PASSWORD=android
mkdir -p "${DEBUG_KEYSTORE_FILE}"
"${KEYTOOL}" \
    -keyalg RSA \
    -genkeypair \
    -alias androiddebugkey \
    -keypass "${ANDROID}" \
    -keystore "${DEBUG_KEYSTORE_FILE}" \
    -storepass "${ANDROID}" \
    -dname "CN=Android Debug,O=Android,C=US" \
    -validity 9999 \
    -deststoretype pkcs12
```

7. In `Godot > Editor > Editor Settings > Export > Android` set the `Android SDK Path` and `Debug Keystore`.

8. In `Godot > Project` click `Install Android Build Template...`.

9. In `Godot > AssetLib` search for and install `Godot OpenXR Vendors plugin`.

10. In `Godot > Project > Project Settings > Plugins` search for and install `Godot OpenXR Vendors plugin`.

