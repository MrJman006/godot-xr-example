This page contains the steps to create a Godot project capable of running on a Meta Quest 2.

## References

1. https://www.youtube.com/watch?v=7XWyZblSnZA<br/>
2. https://forum.godotengine.org/t/is-there-a-way-to-get-unreal-style-mouse-navigation/27110/2
3. https://docs.godotengine.org/en/stable/tutorials/xr/index.html<br/>
4. https://docs.godotengine.org/en/stable/tutorials/xr/setting_up_xr.html<br/>
5. https://github.com/godotengine/godot/issues/84855#issuecomment-1936139765<br/>

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

    2. Set the `Tracker` property of `LeftHand` to `left_hand`.

    3. Set the `Transform > Position` property of `LeftHand` to `xyz(-0.5, 1.0, -0.5)`.

    4. Rename `LeftHand > MeshInstance3D` to `LeftHandMesh`.

    5. Set the `Mesh` property of `LeftHandMesh` to `New BoxMesh`.

    6. Set the `Transform > Scale` property of `LeftHandMesh` to `xyz(0.1, 0.1, 0.1)`.

10. Configure the second `XRController3D` instance to be a right hand controller.

    1. Rename `XRConroller3D` to `RightHand`.

    2. Set the `Tracker` property of `RightHand` to `right_hand`.

    3. Set the `Transform > Position` property of `RightHand` to `xyz(0.5, 1.0, -0.5)`.

    4. Rename `RightHand > MeshInstance3D` to `RightHandMesh`.

    5. Set the `Mesh` property of `RightHandMesh` to `New BoxMesh`.

    6. Set the `Transform > Scale` property of `RightHandMesh` to `xyz(0.1, 0.1, 0.1)`.

11. Set the `Transform > Position` property of `DirectionalLight3D` to `xyz(0.0, 3.0, 0.0)`.

12. Set the `Transform > Rotation` property of `DirectionalLight3D` to `xyz(-45.0, 0.0, 0.0)`.

13. Rename `XRSandbox > MeshInstance3D` to `FloorMesh`.

14. Set the `Mesh` property of `FloorMesh` to `New PlaneMesh`.

15. Create a GD script attached to `XRSandbox` called `XrSandbox.gd`. Paste the contents of the code block below into the script.

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

16. Setup your Meta Quest 2 to work with SteamVR.

    1. Connect your computer and Meta Quest 2 to the same internet. Wired internet to your computer is recommended, but will work with wireless.

    2. Install the app `Steam Link` on the quest headset.

    3. Install `Steam VR` on your desktop.

    4. Plug your quest headset into the computer.

    5. Launch `Steam VR`.

    6. Connect to `Steam VR` via `Steam Link` on the quest headset. You should see a "room" that looks different from the default Meta Quest 2 room.

17. In Godot, click the `Run Project` button in the top right hand corner of Godot.

18. You should see yourself in the `XRSandbox` scene.
