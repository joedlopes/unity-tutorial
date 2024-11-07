# AR Touch Gestures in Unity with XR Interaction Toolkit

## Introduction to Touch Gestures
The XR Interaction Toolkit provides a comprehensive system for handling touch gestures in AR applications through the `TouchscreenGestureInputController`. This component enables natural interactions with AR content using common mobile gestures.

## References
For more detailed information on AR gestures and the XR Interaction Toolkit, please refer to the [Unity XR Interaction Toolkit Documentation](https://docs.unity3d.com/Packages/com.unity.xr.interaction.toolkit@3.0/manual/ar-gestures.html).


## Setting Up Touch Gestures
First, ensure your scene has:
1. XR Origin
2. AR Session
3. AR Session Origin
4. Event System with XR UI Input Module

Add the `TouchscreenGestureInputController` component to your XR Origin or a dedicated input GameObject.

## Gesture Types and Implementation

### 1. Tap Gesture
A simple touch and release interaction.

```csharp
using UnityEngine;
using UnityEngine.XR.Interaction.Toolkit.AR;
using UnityEngine.XR.Interaction.Toolkit;

public class TapGestureHandler : MonoBehaviour
{
    private ARGestureInteractor gestureInteractor;

    void Awake()
    {
        gestureInteractor = FindObjectOfType<ARGestureInteractor>();
    }

    void OnEnable()
    {
        if (gestureInteractor != null)
            gestureInteractor.tapGestureRecognizer.onGestureStarted += OnTapGesture;
    }

    void OnDisable()
    {
        if (gestureInteractor != null)
            gestureInteractor.tapGestureRecognizer.onGestureStarted -= OnTapGesture;
    }

    private void OnTapGesture(TapGesture gesture)
    {
        if (gesture.targetObject == gameObject)
        {
            Debug.Log("Object tapped!");
            // Add your tap response here
        }
    }
}
```

### 2. Drag Gesture
Enables moving objects with single-finger drag.

```csharp
using UnityEngine;
using UnityEngine.XR.Interaction.Toolkit.AR;
using UnityEngine.XR.Interaction.Toolkit;

public class DragGestureHandler : MonoBehaviour
{
    private ARGestureInteractor gestureInteractor;
    private Vector3 initialPosition;

    void Awake()
    {
        gestureInteractor = FindObjectOfType<ARGestureInteractor>();
    }

    void OnEnable()
    {
        if (gestureInteractor != null)
        {
            gestureInteractor.dragGestureRecognizer.onGestureStarted += OnDragStart;
            gestureInteractor.dragGestureRecognizer.onGestureUpdated += OnDragUpdate;
        }
    }

    void OnDisable()
    {
        if (gestureInteractor != null)
        {
            gestureInteractor.dragGestureRecognizer.onGestureStarted -= OnDragStart;
            gestureInteractor.dragGestureRecognizer.onGestureUpdated -= OnDragUpdate;
        }
    }

    private void OnDragStart(DragGesture gesture)
    {
        if (gesture.targetObject == gameObject)
        {
            initialPosition = transform.position;
        }
    }

    private void OnDragUpdate(DragGesture gesture)
    {
        if (gesture.targetObject == gameObject)
        {
            transform.position = initialPosition + gesture.delta;
        }
    }
}
```

### 3. Pinch Gesture
Used for scaling objects.

```csharp
using UnityEngine;
using UnityEngine.XR.Interaction.Toolkit.AR;
using UnityEngine.XR.Interaction.Toolkit;

public class PinchGestureHandler : MonoBehaviour
{
    private ARGestureInteractor gestureInteractor;
    private Vector3 initialScale;

    void Awake()
    {
        gestureInteractor = FindObjectOfType<ARGestureInteractor>();
    }

    void OnEnable()
    {
        if (gestureInteractor != null)
        {
            gestureInteractor.pinchGestureRecognizer.onGestureStarted += OnPinchStart;
            gestureInteractor.pinchGestureRecognizer.onGestureUpdated += OnPinchUpdate;
        }
    }

    void OnDisable()
    {
        if (gestureInteractor != null)
        {
            gestureInteractor.pinchGestureRecognizer.onGestureStarted -= OnPinchStart;
            gestureInteractor.pinchGestureRecognizer.onGestureUpdated -= OnPinchUpdate;
        }
    }

    private void OnPinchStart(PinchGesture gesture)
    {
        if (gesture.targetObject == gameObject)
        {
            initialScale = transform.localScale;
        }
    }

    private void OnPinchUpdate(PinchGesture gesture)
    {
        if (gesture.targetObject == gameObject)
        {
            transform.localScale = initialScale * gesture.gap;
        }
    }
}
```

### 4. Twist Gesture
Enables rotation of objects.

```csharp
using UnityEngine;
using UnityEngine.XR.Interaction.Toolkit.AR;
using UnityEngine.XR.Interaction.Toolkit;

public class TwistGestureHandler : MonoBehaviour
{
    private ARGestureInteractor gestureInteractor;
    private Quaternion initialRotation;

    void Awake()
    {
        gestureInteractor = FindObjectOfType<ARGestureInteractor>();
    }

    void OnEnable()
    {
        if (gestureInteractor != null)
        {
            gestureInteractor.twistGestureRecognizer.onGestureStarted += OnTwistStart;
            gestureInteractor.twistGestureRecognizer.onGestureUpdated += OnTwistUpdate;
        }
    }

    void OnDisable()
    {
        if (gestureInteractor != null)
        {
            gestureInteractor.twistGestureRecognizer.onGestureStarted -= OnTwistStart;
            gestureInteractor.twistGestureRecognizer.onGestureUpdated -= OnTwistUpdate;
        }
    }

    private void OnTwistStart(TwistGesture gesture)
    {
        if (gesture.targetObject == gameObject)
        {
            initialRotation = transform.rotation;
        }
    }

    private void OnTwistUpdate(TwistGesture gesture)
    {
        if (gesture.targetObject == gameObject)
        {
            float rotationAngle = gesture.deltaRotation;
            transform.rotation = initialRotation * Quaternion.Euler(0, rotationAngle, 0);
        }
    }
}
```

### 5. Two-Finger Drag Gesture
Used for advanced object manipulation.

```csharp
using UnityEngine;
using UnityEngine.XR.Interaction.Toolkit.AR;
using UnityEngine.XR.Interaction.Toolkit;

public class TwoFingerDragHandler : MonoBehaviour
{
    private ARGestureInteractor gestureInteractor;
    private Vector3 initialPosition;

    void Awake()
    {
        gestureInteractor = FindObjectOfType<ARGestureInteractor>();
    }

    void OnEnable()
    {
        if (gestureInteractor != null)
        {
            gestureInteractor.twoFingerDragGestureRecognizer.onGestureStarted += OnTwoFingerDragStart;
            gestureInteractor.twoFingerDragGestureRecognizer.onGestureUpdated += OnTwoFingerDragUpdate;
        }
    }

    void OnDisable()
    {
        if (gestureInteractor != null)
        {
            gestureInteractor.twoFingerDragGestureRecognizer.onGestureStarted -= OnTwoFingerDragStart;
            gestureInteractor.twoFingerDragGestureRecognizer.onGestureUpdated -= OnTwoFingerDragUpdate;
        }
    }

    private void OnTwoFingerDragStart(TwoFingerDragGesture gesture)
    {
        if (gesture.targetObject == gameObject)
        {
            initialPosition = transform.position;
        }
    }

    private void OnTwoFingerDragUpdate(TwoFingerDragGesture gesture)
    {
        if (gesture.targetObject == gameObject)
        {
            transform.position = initialPosition + gesture.delta;
        }
    }
}
```

## Best Practices
1. Always check for null references and gesture state.
2. Use proper error handling.
3. Consider implementing gesture conflict resolution.
4. Add visual feedback for gesture recognition.
5. Test on different device sizes and resolutions.

## Implementation Notes
- Attach these scripts to objects that need gesture interaction.
- Ensure proper layer setup for raycasting.
- Consider adding gesture visualization for debugging.
- Implement proper cleanup in `OnDisable`/`OnDestroy`.



## Handling Screen Taps to Select Objects

In this section, we will create a simple AR application using AR Foundation to handle screen taps and select existing objects in the scene. When an object is selected, it will change its color to red.

### Setting Up the Scene

1. Create a new Unity 3D project.
2. Import the AR Foundation package from the Unity Package Manager.
3. Set up an AR Session and AR Session Origin in your scene.
4. Add a few 3D objects to the scene that you want to be selectable.

### Implementing the Tap to Select Feature

Create a new script called `ScreenSpaceSelectInput.cs` and add the following code:

```csharp
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.Interaction.Toolkit.Inputs.Readers;

public class ScreenSpaceSelectInput : MonoBehaviour
{
    [SerializeField]
    XRInputValueReader<Vector2> m_TapStartPositionInput = new XRInputValueReader<Vector2>("Tap Start Position");

    public XRInputValueReader<Vector2> tapStartPositionInput
    {
        get => m_TapStartPositionInput;
        set => XRInputReaderUtility.SetInputProperty(ref m_TapStartPositionInput, value, this);
    }

    private ARRaycastManager raycastManager;

    void Awake()
    {
        raycastManager = GetComponent<ARRaycastManager>();
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

### Adding the Script to the Scene

1. Attach the `ScreenSpaceSelectInput` script to the AR Session Origin GameObject.
2. Ensure the AR Session Origin has an `ARRaycastManager` component.

### Testing the Application

1. Build and run the application on an AR-supported device.
2. Tap on objects in the scene to select them and observe the color change to red.

### Conclusion

By following these steps, you have created a simple AR application that handles screen taps to select objects in the scene using AR Foundation. When an object is selected, it changes its color to red, providing visual feedback for the selection. You can extend this functionality by adding more interactions based on the selected objects.



## Example

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
        if (tapStartPositionInput != null && tapStartPositionInput.TryReadValue(out Vector2 tapPosition))
        {
            HandleTap(tapPosition);
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