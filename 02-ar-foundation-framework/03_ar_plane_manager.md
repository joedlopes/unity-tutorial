# AR Plane Manager

The AR Plane Manager is a crucial component in Unity's AR Foundation framework. It is responsible for detecting and tracking planar surfaces in the real world, such as floors, walls, and tables. These surfaces are then represented as AR planes in the AR scene.

## Setting Up AR Plane Manager

To use the AR Plane Manager, you need to follow these steps:

1. **Install AR Foundation Package**: Ensure that you have the AR Foundation package installed in your Unity project. You can do this via the Package Manager.

2. **Add AR Plane Manager Component**: Attach the `ARPlaneManager` component to a GameObject in your scene. Typically, this is done on the same GameObject that has the `ARSessionOrigin` component.

3. **Configure AR Plane Prefab**: Create a prefab for the AR planes. This prefab will be instantiated for each detected plane. You can customize this prefab to include visual indicators, such as different colors for different planes.

## Example: Detectable Planes in Green and Red

In this example, we will create a script that changes the color of detected planes to green and red and adds them to a list.

### Step 1: Create Plane Prefab

1. Create a new GameObject in your scene and name it `ARPlanePrefab`.
2. Add a `MeshRenderer` component to the `ARPlanePrefab`.
3. Create two materials, one green and one red, and assign them to the `MeshRenderer` component.

### Step 2: Create AR Plane Manager Script

Create a new C# script named `ARPlaneManagerScript` and attach it to the GameObject with the `ARPlaneManager` component.

```csharp
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;

public class ARPlaneManagerScript : MonoBehaviour
{
    public ARPlaneManager arPlaneManager;
    public Material greenMaterial;
    public Material redMaterial;

    private List<ARPlane> greenPlanes = new List<ARPlane>();
    private List<ARPlane> redPlanes = new List<ARPlane>();

    void OnEnable()
    {
        arPlaneManager.planesChanged += OnPlanesChanged;
    }

    void OnDisable()
    {
        arPlaneManager.planesChanged -= OnPlanesChanged;
    }

    void OnPlanesChanged(ARPlanesChangedEventArgs args)
    {
        foreach (var plane in args.added)
        {
            if (plane.alignment == PlaneAlignment.HorizontalUp)
            {
                plane.GetComponent<MeshRenderer>().material = greenMaterial;
                greenPlanes.Add(plane);
            }
            else
            {
                plane.GetComponent<MeshRenderer>().material = redMaterial;
                redPlanes.Add(plane);
            }
        }
    }
}
```

### Step 3: Assign Materials and AR Plane Manager

1. In the Unity Editor, select the GameObject with the `ARPlaneManager` component.
2. Assign the `GreenMaterial` and `RedMaterial` to the corresponding fields in the `ARPlaneManagerScript` component.
3. Drag the `ARPlaneManager` component to the `arPlaneManager` field in the `ARPlaneManagerScript` component.

Now, when you run your AR application, detected horizontal planes will be colored green, and other planes will be colored red. These planes will also be added to their respective lists for further processing.



## Example 1: Spawning Prefab on Detected Planes

In this example, we will create a script that spawns a prefab object on top of a detected plane when the user taps on the screen using the XR Interaction Toolkit events.

### Step 1: Create the Prefab

1. Create a new GameObject in your scene and name it `SpawnedObject`.
2. Customize this GameObject to represent the object you want to spawn (e.g., a cube or a sphere).
3. Create a prefab from this GameObject by dragging it into the `Assets` folder.

### Step 2: Create AR Plane Spawner Script

Create a new C# script named `ARPlaneSpawner` and attach it to the GameObject with the `ARPlaneManager` component.

```csharp
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.Interaction.Toolkit.Inputs.Readers;

public class ARPlaneSpawner : MonoBehaviour
{
    [SerializeField]
    XRInputValueReader<Vector2> m_TapStartPositionInput = new XRInputValueReader<Vector2>("Tap Start Position");

    public XRInputValueReader<Vector2> tapStartPositionInput
    {
        get => m_TapStartPositionInput;
        set => XRInputReaderUtility.SetInputProperty(ref m_TapStartPositionInput, value, this);
    }

    public ARPlaneManager arPlaneManager;
    public GameObject spawnedObjectPrefab;

    private ARRaycastManager raycastManager;

    void Awake()
    {
        raycastManager = GetComponent<ARRaycastManager>();
    }

    void OnEnable()
    {
        arPlaneManager.planesChanged += OnPlanesChanged;
    }

    void OnDisable()
    {
        arPlaneManager.planesChanged -= OnPlanesChanged;
    }

    void OnPlanesChanged(ARPlanesChangedEventArgs args)
    {
        // Handle plane changes if needed
    }

    void Update()
    {
        if (tapStartPositionInput.TryReadValue(out Vector2 tapPosition))
        {
            HandleTap(tapPosition);
        }
    }

    void HandleTap(Vector2 tapPosition)
    {
        List<ARRaycastHit> hits = new List<ARRaycastHit>();
        if (raycastManager.Raycast(tapPosition, hits, UnityEngine.XR.ARSubsystems.TrackableType.PlaneWithinPolygon))
        {
            Pose hitPose = hits[0].pose;
            Instantiate(spawnedObjectPrefab, hitPose.position, hitPose.rotation);
        }
    }
}
```


### Step 3: Assign Prefab and AR Plane Manager

1. In the Unity Editor, select the GameObject with the `ARPlaneManager` component.
2. Assign the `SpawnedObjectPrefab` to the corresponding field in the `ARPlaneSpawner` component.
3. Drag the `ARPlaneManager` component to the `arPlaneManager` field in the `ARPlaneSpawner` component.
4. Assign the `Tap Start Position` input action to the `Tap Start Position Input` field in the `ARPlaneSpawner` component.

Now, when you run your AR application, a prefab object will be spawned on top of a detected plane when the user taps on the screen using the XR Interaction Toolkit events.



## Example 2: Instantianting Single Object at the time

If you ran the previous example, you might have noticed that multiple boxes were instantiated with each tap. In this example, we will fix this by adding a `isTapHeld` variable (`bool`) to check whether the tap action is pressed or not.


```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.Interaction.Toolkit.Inputs.Readers;

public class TapBoxCreator : MonoBehaviour
{
    [SerializeField]
    XRInputValueReader<Vector2> m_TapStartPositionInput = new XRInputValueReader<Vector2>("Tap Start Position");

    public XRInputValueReader<Vector2> tapStartPositionInput
    {
        get => m_TapStartPositionInput;
        set => XRInputReaderUtility.SetInputProperty(ref m_TapStartPositionInput, value, this);
    }

    public ARPlaneManager arPlaneManager;
    public GameObject spawnedObjectPrefab;

    private ARRaycastManager raycastManager;
    private bool isRaycastManagerInitialized = false;
    private bool isTapHeld = false;

    void Awake()
    {
        raycastManager = GetComponent<ARRaycastManager>();
        if (raycastManager != null)
        {
            isRaycastManagerInitialized = true;
        }
        else
        {
            Debug.LogError("ARRaycastManager component is missing. Please attach it to the GameObject.");
        }
    }

    void OnEnable()
    {
        if (arPlaneManager != null)
        {
            arPlaneManager.planesChanged += OnPlanesChanged;
        }
        else
        {
            Debug.LogError("ARPlaneManager is not set. Please assign it in the Inspector.");
        }
    }

    void OnDisable()
    {
        if (arPlaneManager != null)
        {
            arPlaneManager.planesChanged -= OnPlanesChanged;
        }
    }

    void OnPlanesChanged(ARPlanesChangedEventArgs args)
    {
        // Handle plane changes if needed
    }

    void Update()
    {
        if (tapStartPositionInput != null)
        {
            if (tapStartPositionInput.TryReadValue(out Vector2 tapPosition))
            {
                if (!isTapHeld)
                {
                    HandleTap(tapPosition);
                    isTapHeld = true;
                }
            }
            else if (isTapHeld)
            {
                // Reset when the tap is released
                isTapHeld = false;
            }
        }
    }

    void HandleTap(Vector2 tapPosition)
    {
        if (!isRaycastManagerInitialized)
        {
            Debug.LogError("ARRaycastManager is not initialized.");
            return;
        }

        List<ARRaycastHit> hits = new List<ARRaycastHit>();
        if (raycastManager.Raycast(tapPosition, hits, UnityEngine.XR.ARSubsystems.TrackableType.PlaneWithinPolygon))
        {
            Pose hitPose = hits[0].pose;
            Instantiate(spawnedObjectPrefab, hitPose.position, hitPose.rotation);
        }
        else
        {
            Debug.Log("No plane hit detected at tap position.");
        }
    }
}
```
