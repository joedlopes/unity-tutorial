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



## Example: Spawning Items on Horizontal Planes

In this example, we will create a script that spawns items in the middle of horizontal planes added to the scene. The user will be able to tap and select one of three items, and a message will be printed to the console when an item is selected.

### Step 1: Create Item Prefabs

1. Create three new GameObjects in your scene and name them `Item1`, `Item2`, and `Item3`.
2. Customize these GameObjects to represent different items (e.g., different shapes or colors).
3. Create prefabs from these GameObjects by dragging them into the `Assets` folder.

### Step 2: Create AR Plane Item Spawner Script

Create a new C# script named `ARPlaneItemSpawner` and attach it to the GameObject with the `ARPlaneManager` component.

```csharp
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;

public class ARPlaneItemSpawner : MonoBehaviour
{
    public ARPlaneManager arPlaneManager;
    public GameObject item1Prefab;
    public GameObject item2Prefab;
    public GameObject item3Prefab;

    private List<GameObject> spawnedItems = new List<GameObject>();

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
                Vector3 spawnPosition = plane.center;
                GameObject spawnedItem = Instantiate(item1Prefab, spawnPosition, Quaternion.identity);
                spawnedItems.Add(spawnedItem);
            }
        }
    }

    void Update()
    {
        if (Input.touchCount > 0)
        {
            Touch touch = Input.GetTouch(0);
            if (touch.phase == TouchPhase.Began)
            {
                Ray ray = Camera.main.ScreenPointToRay(touch.position);
                if (Physics.Raycast(ray, out RaycastHit hit))
                {
                    GameObject selectedItem = hit.transform.gameObject;
                    if (spawnedItems.Contains(selectedItem))
                    {
                        Debug.Log($"{selectedItem.name} selected");
                    }
                }
            }
        }
    }
}
```

### Step 3: Assign Item Prefabs and AR Plane Manager

1. In the Unity Editor, select the GameObject with the `ARPlaneManager` component.
2. Assign the `Item1Prefab`, `Item2Prefab`, and `Item3Prefab` to the corresponding fields in the `ARPlaneItemSpawner` component.
3. Drag the `ARPlaneManager` component to the `arPlaneManager` field in the `ARPlaneItemSpawner` component.

Now, when you run your AR application, items will be spawned in the middle of detected horizontal planes. You can tap on these items to select them, and a message will be printed to the console indicating which item was selected.


## Example: Spawning Prefab on Detected Plane

In this example, we will create a script that spawns a prefab object on top of a detected plane when the user taps on the screen.

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
using UnityEngine.XR.ARSubsystems;

public class ARPlaneSpawner : MonoBehaviour
{
    public ARPlaneManager arPlaneManager;
    public GameObject spawnedObjectPrefab;

    private ARRaycastManager raycastManager;
    private List<ARRaycastHit> hits = new List<ARRaycastHit>();

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
        if (Input.touchCount > 0)
        {
            Touch touch = Input.GetTouch(0);
            if (touch.phase == TouchPhase.Began)
            {
                Vector2 touchPosition = touch.position;
                if (raycastManager.Raycast(touchPosition, hits, TrackableType.PlaneWithinPolygon))
                {
                    Pose hitPose = hits[0].pose;
                    Instantiate(spawnedObjectPrefab, hitPose.position, hitPose.rotation);
                }
            }
        }
    }
}
```

### Step 3: Assign Prefab and AR Plane Manager

1. In the Unity Editor, select the GameObject with the `ARPlaneManager` component.
2. Assign the `SpawnedObjectPrefab` to the corresponding field in the `ARPlaneSpawner` component.
3. Drag the `ARPlaneManager` component to the `arPlaneManager` field in the `ARPlaneSpawner` component.

Now, when you run your AR application, a prefab object will be spawned on top of a detected plane when the user taps on the screen.
