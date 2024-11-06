# Unity 3D: AR Foundation Framework Version 5

## Introduction
Unity 3D is a powerful cross-platform game engine used for developing 2D, 3D, VR, and AR experiences. The AR Foundation Framework is a Unity package that provides a unified workflow for augmented reality (AR) development across multiple platforms.

## AR Foundation Framework Version 5
The AR Foundation Framework version 5 introduces several new features and improvements to enhance AR development:

### Key Features
- **Cross-Platform Support**: Seamlessly develop AR applications for both iOS and Android using a single codebase.
- **Enhanced Tracking**: Improved tracking capabilities for more accurate and stable AR experiences.
- **New AR Features**: Support for new AR features such as occlusion, meshing, and environment probes.
- **Performance Optimizations**: Optimized performance for smoother and more responsive AR applications.

### Getting Started
To get started with AR Foundation Framework version 5:
1. **Install Unity**: Download and install the latest version of Unity.
2. **Add AR Foundation Package**: Open the Unity Package Manager and add the AR Foundation package.
3. **Configure AR Settings**: Set up your project for AR development by configuring the necessary AR settings.
4. **Build and Deploy**: Build and deploy your AR application to your target device.

### Resources
- [Unity AR Foundation Documentation](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@latest)
- [Unity Learn: AR Development](https://learn.unity.com/)
- [AR Foundation Samples](https://github.com/Unity-Technologies/arfoundation-samples)



## Main Concepts of AR Foundation Framework

### AR Session
The AR Session component is responsible for managing the lifecycle of an AR experience. It handles the initialization, updates, and termination of the AR session.

### AR Session Origin
The AR Session Origin component defines the origin point of the AR session. It is used to transform AR data (such as position and orientation) into Unity's coordinate system.

### AR Camera
The AR Camera is a specialized camera that captures the real-world environment and overlays AR content on top of it. It is typically a child of the AR Session Origin.

### AR Plane Manager
The AR Plane Manager component detects and tracks horizontal and vertical planes in the real world. These planes can be used to place AR content.

### AR Raycast Manager
The AR Raycast Manager component performs raycasting to detect surfaces and objects in the real world. It is used to interact with AR content by tapping or pointing.

### AR Anchor Manager
The AR Anchor Manager component allows you to create and manage anchors in the real world. Anchors are fixed points that can be used to place AR content.


## Working with the AR Plane Manager

The AR Plane Manager is a crucial component for detecting and tracking planes in the real world. These planes can be used to place AR content accurately. Here's how you can work with the AR Plane Manager to detect planes:

### Adding the AR Plane Manager

1. **Create a New GameObject**: In your Unity scene, create a new empty GameObject and name it `AR Plane Manager`.
2. **Attach AR Plane Manager Component**: Select the `AR Plane Manager` GameObject and click on `Add Component`. Search for `AR Plane Manager` and add it to the GameObject.

### Configuring the AR Plane Manager

1. **Plane Prefab**: Create a prefab for the planes that will be detected. This can be a simple plane mesh with a material applied to it.
2. **Assign Plane Prefab**: In the AR Plane Manager component, assign the plane prefab to the `Plane Prefab` field.

### Detecting Planes

The AR Plane Manager will automatically detect horizontal and vertical planes in the real world. You can handle plane detection events by subscribing to the `planesChanged` event.

```csharp
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using System.Collections.Generic;

public class PlaneDetection : MonoBehaviour
{
    [SerializeField]
    private ARPlaneManager arPlaneManager;

    void OnEnable()
    {
        arPlaneManager.planesChanged += OnPlanesChanged;
    }

    void OnDisable()
    {
        arPlaneManager.planesChanged -= OnPlanesChanged;
    }

    void OnPlanesChanged(ARPlanesChangedEventArgs eventArgs)
    {
        foreach (ARPlane addedPlane in eventArgs.added)
        {
            Debug.Log("Plane added: " + addedPlane.trackableId);
        }

        foreach (ARPlane updatedPlane in eventArgs.updated)
        {
            Debug.Log("Plane updated: " + updatedPlane.trackableId);
        }

        foreach (ARPlane removedPlane in eventArgs.removed)
        {
            Debug.Log("Plane removed: " + removedPlane.trackableId);
        }
    }
}
```

### Visualizing Detected Planes

To visualize the detected planes, ensure that your plane prefab has a visible mesh or material. The AR Plane Manager will instantiate this prefab for each detected plane.

By following these steps, you can effectively detect and visualize planes in your AR application using the AR Plane Manager.


## Managing and Joining Multiple Planes

In some AR applications, you may need to manage and join multiple planes that are detected as neighbors. This can be useful for creating larger surfaces or for more complex interactions. Here's how you can manage and join multiple planes:

### Detecting Neighboring Planes

To detect neighboring planes, you can use the AR Plane Manager's `planesChanged` event to keep track of all detected planes and their boundaries.

```csharp
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using System.Collections.Generic;

public class PlaneManager : MonoBehaviour
{
    [SerializeField]
    private ARPlaneManager arPlaneManager;

    private List<ARPlane> allPlanes = new List<ARPlane>();

    void OnEnable()
    {
        arPlaneManager.planesChanged += OnPlanesChanged;
    }

    void OnDisable()
    {
        arPlaneManager.planesChanged -= OnPlanesChanged;
    }

    void OnPlanesChanged(ARPlanesChangedEventArgs eventArgs)
    {
        foreach (ARPlane addedPlane in eventArgs.added)
        {
            allPlanes.Add(addedPlane);
        }

        foreach (ARPlane removedPlane in eventArgs.removed)
        {
            allPlanes.Remove(removedPlane);
        }

        JoinNeighboringPlanes();
    }

    void JoinNeighboringPlanes()
    {
        // Implement logic to join neighboring planes
    }
}
```

### Joining Neighboring Planes

To join neighboring planes, you need to check the boundaries of each plane and determine if they are close enough to be considered neighbors. If they are, you can merge their boundaries to create a larger plane.

```csharp
void JoinNeighboringPlanes()
{
    for (int i = 0; i < allPlanes.Count; i++)
    {
        for (int j = i + 1; j < allPlanes.Count; j++)
        {
            if (ArePlanesNeighbors(allPlanes[i], allPlanes[j]))
            {
                MergePlanes(allPlanes[i], allPlanes[j]);
            }
        }
    }
}

bool ArePlanesNeighbors(ARPlane plane1, ARPlane plane2)
{
    // Implement logic to determine if planes are neighbors
    return false;
}

void MergePlanes(ARPlane plane1, ARPlane plane2)
{
    // Implement logic to merge planes
}
```

### Visualizing Merged Planes

To visualize the merged planes, you can update the plane prefab or create a new prefab that represents the merged plane.

```csharp
void MergePlanes(ARPlane plane1, ARPlane plane2)
{
    // Example logic to merge planes
    Vector3 mergedCenter = (plane1.center + plane2.center) / 2;
    Vector3 mergedSize = plane1.size + plane2.size;

    // Update plane1 to represent the merged plane
    plane1.center = mergedCenter;
    plane1.size = mergedSize;

    // Disable plane2
    plane2.gameObject.SetActive(false);
}
```

By following these steps, you can manage and join multiple planes that are detected as neighbors, creating larger and more complex surfaces for your AR application.




## Using the AR Raycast Manager to Spawn Objects on Detected Planes

The AR Raycast Manager is a powerful component that allows you to interact with the real world by detecting surfaces and objects through raycasting. In this section, we will explain how to use the AR Raycast Manager to spawn objects on detected planes based on user taps. We will utilize the XR Interaction Toolkit and the new Input System to handle user input for raycasting.

### Setting Up the XR Interaction Toolkit and Input System

1. **Install XR Interaction Toolkit**: Open the Unity Package Manager and install the XR Interaction Toolkit package.
2. **Install Input System**: Similarly, install the Input System package from the Unity Package Manager.
3. **Enable Input System**: Go to `Edit > Project Settings > Player`, and under the `Other Settings` tab, change the `Active Input Handling` to `Both`.

### Configuring the AR Raycast Manager

1. **Create a New GameObject**: In your Unity scene, create a new empty GameObject and name it `AR Raycast Manager`.
2. **Attach AR Raycast Manager Component**: Select the `AR Raycast Manager` GameObject and click on `Add Component`. Search for `AR Raycast Manager` and add it to the GameObject.

### Setting Up the Input Action

1. **Create Input Action Asset**: Right-click in the Project window and select `Create > Input Actions`. Name the asset `ARInputActions`.
2. **Define Tap Action**: Open the `ARInputActions` asset, create a new action map named `AR`, and add an action named `Tap`. Set the action type to `Pass Through` and the control type to `Button`.
3. **Bind Tap Action**: Add a binding to the `Tap` action and set it to `Touchscreen/PrimaryTouch/Press`.

### Implementing the Raycast Logic

1. **Create a New Script**: Create a new C# script named `ARRaycastSpawner` and attach it to the `AR Raycast Manager` GameObject.
2. **Script Implementation**: Implement the raycast logic to detect planes and spawn objects.

```csharp
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.InputSystem;
using UnityEngine.XR.Interaction.Toolkit.AR;
using System.Collections.Generic;

public class ARRaycastSpawner : MonoBehaviour
{
    [SerializeField]
    private GameObject objectToSpawn;

    private ARRaycastManager arRaycastManager;
    private List<ARRaycastHit> hits = new List<ARRaycastHit>();
    private ARInputActions arInputActions;

    void Awake()
    {
        arRaycastManager = GetComponent<ARRaycastManager>();
        arInputActions = new ARInputActions();
    }

    void OnEnable()
    {
        arInputActions.AR.Tap.performed += OnTap;
        arInputActions.Enable();
    }

    void OnDisable()
    {
        arInputActions.AR.Tap.performed -= OnTap;
        arInputActions.Disable();
    }

    void OnTap(InputAction.CallbackContext context)
    {
        Vector2 touchPosition = context.ReadValue<Vector2>();

        if (arRaycastManager.Raycast(touchPosition, hits, UnityEngine.XR.ARSubsystems.TrackableType.PlaneWithinPolygon))
        {
            Pose hitPose = hits[0].pose;
            Instantiate(objectToSpawn, hitPose.position, hitPose.rotation);
        }
    }
}
```

### Assigning the Object to Spawn

1. **Create a Prefab**: Create a prefab for the object you want to spawn (e.g., a 3D model).
2. **Assign Prefab**: In the `ARRaycastSpawner` component, assign the prefab to the `Object To Spawn` field.

### Testing the Implementation

1. **Build and Deploy**: Build and deploy your AR application to your target device.
2. **Interact with AR**: Tap on detected planes in the real world to spawn objects at the tap location.

By following these steps, you can effectively use the AR Raycast Manager to spawn objects on detected planes based on user taps, leveraging the XR Interaction Toolkit and the new Input System for handling user input.



## Selecting Objects with Raycast by Taps

In this section, we will demonstrate how to select objects in the scene using raycast by tapping on the screen. We will use the XR Interaction Toolkit, AR Foundation Framework, and the new Input System to handle user input for raycasting.

### Setting Up the Scene

1. **Create a New GameObject**: In your Unity scene, create a new empty GameObject and name it `AR Object Selector`.
2. **Attach AR Raycast Manager Component**: Select the `AR Object Selector` GameObject and click on `Add Component`. Search for `AR Raycast Manager` and add it to the GameObject.

### Configuring the Input Action

1. **Create Input Action Asset**: Right-click in the Project window and select `Create > Input Actions`. Name the asset `ARInputActions`.
2. **Define Select Action**: Open the `ARInputActions` asset, create a new action map named `AR`, and add an action named `Select`. Set the action type to `Pass Through` and the control type to `Button`.
3. **Bind Select Action**: Add a binding to the `Select` action and set it to `Touchscreen/PrimaryTouch/Press`.

### Implementing the Selection Logic

1. **Create a New Script**: Create a new C# script named `ARObjectSelector` and attach it to the `AR Object Selector` GameObject.
2. **Script Implementation**: Implement the raycast logic to detect and select objects.

```csharp
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.InputSystem;
using UnityEngine.XR.Interaction.Toolkit.AR;
using System.Collections.Generic;

public class ARObjectSelector : MonoBehaviour
{
    private ARRaycastManager arRaycastManager;
    private List<ARRaycastHit> hits = new List<ARRaycastHit>();
    private ARInputActions arInputActions;

    void Awake()
    {
        arRaycastManager = GetComponent<ARRaycastManager>();
        arInputActions = new ARInputActions();
    }

    void OnEnable()
    {
        arInputActions.AR.Select.performed += OnSelect;
        arInputActions.Enable();
    }

    void OnDisable()
    {
        arInputActions.AR.Select.performed -= OnSelect;
        arInputActions.Disable();
    }

    void OnSelect(InputAction.CallbackContext context)
    {
        Vector2 touchPosition = context.ReadValue<Vector2>();

        if (arRaycastManager.Raycast(touchPosition, hits, UnityEngine.XR.ARSubsystems.TrackableType.PlaneWithinPolygon))
        {
            Pose hitPose = hits[0].pose;
            SelectObject(hitPose.position);
        }
    }

    void SelectObject(Vector3 position)
    {
        Ray ray = Camera.main.ScreenPointToRay(position);
        RaycastHit hit;

        if (Physics.Raycast(ray, out hit))
        {
            GameObject selectedObject = hit.collider.gameObject;
            Debug.Log("Selected object: " + selectedObject.name);
            // Implement additional logic for selected object
        }
    }
}
```

### Testing the Implementation

1. **Build and Deploy**: Build and deploy your AR application to your target device.
2. **Interact with AR**: Tap on objects in the real world to select them and see the selection logic in action.

By following these steps, you can effectively select objects in the scene using raycast by tapping on the screen, leveraging the XR Interaction Toolkit, AR Foundation Framework, and the new Input System for handling user input.


## Detecting Markers or Images Using AR Foundation Image Tracking

AR Foundation provides robust image tracking capabilities that allow you to detect and track 2D images in the real world. This can be useful for creating AR experiences that respond to specific images or markers. Here's how you can set up and use the AR Foundation Image Tracking system:

### Setting Up Image Tracking

1. **Prepare Your Images**: Ensure that the images you want to track are high quality and have distinct features. Save these images in your Unity project's `Assets` folder.

2. **Create a Reference Image Library**:
    - In the Project window, right-click and select `Create > XR > Reference Image Library`.
    - Name the library (e.g., `MyImageLibrary`).
    - Open the library and add your images by clicking the `Add Image` button. Set the `Name` and `Width` for each image.

3. **Configure AR Session Origin**:
    - Select the `AR Session Origin` GameObject in your scene.
    - Click `Add Component` and add the `ARTrackedImageManager` component.
    - Assign the `Reference Library` field to the image library you created.

### Implementing Image Tracking Logic

1. **Create a New Script**: Create a new C# script named `ImageTrackingHandler` and attach it to the `AR Session Origin` GameObject.

2. **Script Implementation**: Implement the logic to handle image tracking events.

```csharp
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;
using System.Collections.Generic;

public class ImageTrackingHandler : MonoBehaviour
{
    [SerializeField]
    private GameObject trackedImagePrefab;

    private ARTrackedImageManager arTrackedImageManager;

    void Awake()
    {
        arTrackedImageManager = GetComponent<ARTrackedImageManager>();
    }

    void OnEnable()
    {
        arTrackedImageManager.trackedImagesChanged += OnTrackedImagesChanged;
    }

    void OnDisable()
    {
        arTrackedImageManager.trackedImagesChanged -= OnTrackedImagesChanged;
    }

    void OnTrackedImagesChanged(ARTrackedImagesChangedEventArgs eventArgs)
    {
        foreach (ARTrackedImage trackedImage in eventArgs.added)
        {
            Instantiate(trackedImagePrefab, trackedImage.transform.position, trackedImage.transform.rotation);
        }

        foreach (ARTrackedImage trackedImage in eventArgs.updated)
        {
            // Update the position and rotation of the tracked image prefab
            trackedImagePrefab.transform.position = trackedImage.transform.position;
            trackedImagePrefab.transform.rotation = trackedImage.transform.rotation;
        }

        foreach (ARTrackedImage trackedImage in eventArgs.removed)
        {
            // Handle the removal of the tracked image
            Destroy(trackedImage.gameObject);
        }
    }
}
```

### Assigning the Tracked Image Prefab

1. **Create a Prefab**: Create a prefab for the object you want to display when an image is tracked (e.g., a 3D model or UI element).
2. **Assign Prefab**: In the `ImageTrackingHandler` component, assign the prefab to the `Tracked Image Prefab` field.

### Testing the Implementation

1. **Build and Deploy**: Build and deploy your AR application to your target device.
2. **Interact with AR**: Point your device's camera at the images you added to the reference image library to see the tracking in action.

By following these steps, you can effectively detect and track markers or images using the AR Foundation Image Tracking system, enhancing your AR application with image-based interactions.