# AR Image Tracking

The AR Image Tracking feature in Unity's AR Foundation framework allows you to detect and track images in the real world. This can be used to spawn objects or trigger events when specific images are recognized.

## Setting Up AR Image Tracking

To use the AR Image Tracking feature, follow these steps:

1. **Install AR Foundation Package**: Ensure that you have the AR Foundation package installed in your Unity project via the Package Manager.
2. **Add AR Tracked Image Manager Component**: Attach the `ARTrackedImageManager` component to a GameObject in your scene, typically the same GameObject that has the `ARSessionOrigin` component.
3. **Configure Reference Image Library**: Create a reference image library containing the images you want to track. This library will be used by the `ARTrackedImageManager` to recognize and track images.

## Example: Spawning Object on Tracked Image

In this example, we will create a script that spawns a prefab object on top of a tracked image.

### Step 1: Create the Prefab

1. Create a new GameObject in your scene and name it `SpawnedObject`.
2. Customize this GameObject to represent the object you want to spawn (e.g., a cube or a sphere).
3. Create a prefab from this GameObject by dragging it into the `Assets` folder.

### Step 2: Create AR Image Tracking Script

Create a new C# script named `ARImageTracking` and attach it to the GameObject with the `ARTrackedImageManager` component.

```csharp
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;

public class ARImageTracking : MonoBehaviour
{
    public ARTrackedImageManager trackedImageManager;
    public GameObject spawnedObjectPrefab;

    private Dictionary<string, GameObject> spawnedObjects = new Dictionary<string, GameObject>();

    void OnEnable()
    {
        trackedImageManager.trackedImagesChanged += OnTrackedImagesChanged;
    }

    void OnDisable()
    {
        trackedImageManager.trackedImagesChanged -= OnTrackedImagesChanged;
    }

    void OnTrackedImagesChanged(ARTrackedImagesChangedEventArgs eventArgs)
    {
        foreach (var trackedImage in eventArgs.added)
        {
            SpawnObject(trackedImage);
        }

        foreach (var trackedImage in eventArgs.updated)
        {
            UpdateObject(trackedImage);
        }

        foreach (var trackedImage in eventArgs.removed)
        {
            RemoveObject(trackedImage);
        }
    }

    void SpawnObject(ARTrackedImage trackedImage)
    {
        var prefab = Instantiate(spawnedObjectPrefab, trackedImage.transform.position, trackedImage.transform.rotation);
        spawnedObjects[trackedImage.referenceImage.name] = prefab;
    }

    void UpdateObject(ARTrackedImage trackedImage)
    {
        if (spawnedObjects.TryGetValue(trackedImage.referenceImage.name, out var prefab))
        {
            prefab.transform.position = trackedImage.transform.position;
            prefab.transform.rotation = trackedImage.transform.rotation;
        }
    }

    void RemoveObject(ARTrackedImage trackedImage)
    {
        if (spawnedObjects.TryGetValue(trackedImage.referenceImage.name, out var prefab))
        {
            Destroy(prefab);
            spawnedObjects.Remove(trackedImage.referenceImage.name);
        }
    }
}
```

### Step 3: Assign Prefab and AR Tracked Image Manager

1. In the Unity Editor, select the GameObject with the `ARTrackedImageManager` component.
2. Assign the `SpawnedObjectPrefab` to the corresponding field in the `ARImageTracking` component.
3. Drag the `ARTrackedImageManager` component to the `trackedImageManager` field in the `ARImageTracking` component.

Now, when you run your AR application, a prefab object will be spawned on top of a tracked image, and it will follow the image as it moves.


## Example: Spawning and Selecting Object on Tracked Image

In this example, we will create a script that spawns a prefab object on top of a tracked image and allows you to select it with a mouse tap.

### Step 1: Create the Prefab

1. Create a new GameObject in your scene and name it `SpawnedObject`.
2. Customize this GameObject to represent the object you want to spawn (e.g., a cube or a sphere).
3. Create a prefab from this GameObject by dragging it into the `Assets` folder.

### Step 2: Create AR Image Tracking Script

Create a new C# script named `ARImageTracking` and attach it to the GameObject with the `ARTrackedImageManager` component.

```csharp
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;

public class ARImageTracking : MonoBehaviour
{
    public ARTrackedImageManager trackedImageManager;
    public GameObject spawnedObjectPrefab;

    private Dictionary<string, GameObject> spawnedObjects = new Dictionary<string, GameObject>();

    void OnEnable()
    {
        trackedImageManager.trackedImagesChanged += OnTrackedImagesChanged;
    }

    void OnDisable()
    {
        trackedImageManager.trackedImagesChanged -= OnTrackedImagesChanged;
    }

    void OnTrackedImagesChanged(ARTrackedImagesChangedEventArgs eventArgs)
    {
        foreach (var trackedImage in eventArgs.added)
        {
            SpawnObject(trackedImage);
        }

        foreach (var trackedImage in eventArgs.updated)
        {
            UpdateObject(trackedImage);
        }

        foreach (var trackedImage in eventArgs.removed)
        {
            RemoveObject(trackedImage);
        }
    }

    void SpawnObject(ARTrackedImage trackedImage)
    {
        var prefab = Instantiate(spawnedObjectPrefab, trackedImage.transform.position, trackedImage.transform.rotation);
        spawnedObjects[trackedImage.referenceImage.name] = prefab;
    }

    void UpdateObject(ARTrackedImage trackedImage)
    {
        if (spawnedObjects.TryGetValue(trackedImage.referenceImage.name, out var prefab))
        {
            prefab.transform.position = trackedImage.transform.position;
            prefab.transform.rotation = trackedImage.transform.rotation;
        }
    }

    void RemoveObject(ARTrackedImage trackedImage)
    {
        if (spawnedObjects.TryGetValue(trackedImage.referenceImage.name, out var prefab))
        {
            Destroy(prefab);
            spawnedObjects.Remove(trackedImage.referenceImage.name);
        }
    }
}
```

### Step 3: Create Screen Space Select Input Script

Create a new C# script named `ScreenSpaceSelectInput` and attach it to the same GameObject with the `ARTrackedImageManager` component.

```csharp
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.XR.ARFoundation;

public class ScreenSpaceSelectInput : MonoBehaviour
{
    private ARRaycastManager raycastManager;

    void Awake()
    {
        raycastManager = GetComponent<ARRaycastManager>();
    }

    void Update()
    {
        if (Input.GetMouseButtonDown(0))
        {
            HandleTap(Input.mousePosition);
        }
    }

    void HandleTap(Vector2 tapPosition)
    {
        List<ARRaycastHit> hits = new List<ARRaycastHit>();
        if (raycastManager.Raycast(tapPosition, hits, UnityEngine.XR.ARSubsystems.TrackableType.PlaneWithinPolygon))
        {
            var hitPose = hits[0].pose;
            var hitObject = hits[0].trackable as ARTrackable;

            if (hitObject != null)
            {
                Renderer renderer = hitObject.GetComponent<Renderer>();
                if (renderer != null)
                {
                    renderer.material.color = Color.red;
                }
            }
        }
    }
}
```

### Step 4: Assign Prefab and AR Tracked Image Manager

1. In the Unity Editor, select the GameObject with the `ARTrackedImageManager` component.
2. Assign the `SpawnedObjectPrefab` to the corresponding field in the `ARImageTracking` component.
3. Drag the `ARTrackedImageManager` component to the `trackedImageManager` field in the `ARImageTracking` component.
4. Ensure the `ScreenSpaceSelectInput` script is attached to the same GameObject.

Now, when you run your AR application, a prefab object will be spawned on top of a tracked image, and you can select it with a mouse tap to change its color.
