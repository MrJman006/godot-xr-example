This page contains the steps to create a Godot project capable of running on a Meta Quest 2.

## References

1. https://www.youtube.com/watch?v=7XWyZblSnZA<br/>
2. https://forum.godotengine.org/t/is-there-a-way-to-get-unreal-style-mouse-navigation/27110/2
2. https://docs.godotengine.org/en/stable/tutorials/xr/index.html<br/>
3. https://docs.godotengine.org/en/stable/tutorials/xr/setting_up_xr.html<br/>
4. https://github.com/godotengine/godot/issues/84855#issuecomment-1936139765<br/>
5. https://docs.godotengine.org/en/stable/tutorials/xr/deploying_to_android.html<br/>
6. https://docs.godotengine.org/en/stable/tutorials/export/exporting_for_android.html<br/>

## Steps

1. Install Godot. Used version 4.2.1.

2. Open the Godot project explorer and create a new project named `Godot XR Example`. Set the `Renderer` to `Compatibility` and `Version Control Metadata` to `Git`.

3. In the Godot project, navigate to `Project > Project Settings > XR > Shaders` and make sure `Enabled > on` is checked.

4. In the Godot project, navigate to `Project > Project Settings > XR > OpenXR` and make sure `Enabled > on` is checked.

5. Save and restart.

6. Create the scene as laid out below.

```
Node3D
|-- XROrigin3D
|   |-- XRCamera3D
|   |-- XRController3D
|   |   `-- MeshInstance3D
|   `-- XRController3D
|       `-- MeshInstance3D
|-- DirectionalLight3D
`-- MeshInstance3D
```

7. Rename `Node3D` to `XRSandbox`. Save the scene as `XrSandbox.tscn`.

8. Set the `Transform > Position` property of `XRCamera3D` to `xyz(0.0, 1.7, 0.0)`.

9. Configure the first `XRController3D` instance to be a left hand controller.

    1. Rename `XRConroller3D` to `LeftHand`.

    1. Set the `Tracker` property of `LeftHand` to `left_hand`.

    1. Set the `Transform > Position` property of `LeftHand` to `xyz(-0.5, 1.0, -0.5)`.

    1. Rename `LeftHand > MeshInstance3D` to `LeftHandMesh`.

    1. Set the `Mesh` property of `LeftHandMesh` to `New BoxMesh`.

    1. Set the `Transform > Scale` property of `LeftHandMesh` to `xyz(0.1, 0.1, 0.1)`.

9. Configure the second `XRController3D` instance to be a right hand controller.

    1. Rename `XRConroller3D` to `RightHand`.

    1. Set the `Tracker` property of `RightHand` to `right_hand`.

    1. Set the `Transform > Position` property of `RightHand` to `xyz(0.5, 1.0, -0.5)`.

    1. Rename `RightHand > MeshInstance3D` to `RightHandMesh`.

    1. Set the `Mesh` property of `RightHandMesh` to `New BoxMesh`.

    1. Set the `Transform > Scale` property of `RightHandMesh` to `xyz(0.1, 0.1, 0.1)`.

16. Set the `Transform > Position` property of `DirectionalLight3D` to `xyz(0.0, 3.0, 0.0)`.

17. Set the `Transform > Rotation` property of `DirectionalLight3D` to `xyz(-45.0, 0.0, 0.0)`.

1. Rename `XRSandbox > MeshInstance3D` to `FloorMesh`.

1. Set the `Mesh` property of `FloorMesh` to `New PlaneMesh`.

16. Create a GD script attached to `XRSandbox` called `XrSandbox.gd`. Paste the contents of the code block below into the script.

```
extends Node3D

var xr_interface: XRInterface

func _ready():
	xr_interface = XRServer.find_interface("OpenXR")
	if xr_interface and xr_interface.is_initialized():
		print("OpenXR initialized successfully")

		# Turn off v-sync!
		DisplayServer.window_set_vsync_mode(DisplayServer.VSYNC_DISABLED)

		# Change our main viewport to output to the HMD
		get_viewport().use_xr = true
	else:
		print("OpenXR not initialized, please check if your headset is connected")
```

1. Setup your Meta Quest 2 to work with SteamVR.

    1. Connect your computer and Meta Quest 2 to the same internet. Wired internet to your computer is recommended, but will work with wireless.

    1. Install the app `Steam Link` on the quest headset.

    1. Install `Steam VR` on your desktop.

    1. Plug your quest headset into the computer.

    1. Launch `Steam VR`.

    1. Connect to `Steam VR` via `Steam Link` on the quest headset. You should see a "room" that looks different from the default Meta Quest 2 room.

1. In Godot, click the `Run Project` button in the top right hand corner of Godot.

1. You should see yourself in the `XRSandbox` scene.



--------------------------------------------------------------------------------
**Archived Text**



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

