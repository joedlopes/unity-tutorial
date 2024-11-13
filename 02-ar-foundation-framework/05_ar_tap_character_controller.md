# Example AR Tap Character Controller

This C# script, `CharacterTapController`, is designed for use in a Unity project that utilizes AR Foundation. The script allows a character to be spawned and controlled via tap inputs on an AR plane. Here's a breakdown of its functionality:

1. **Dependencies and Fields**:
    - The script uses `UnityEngine`, `UnityEngine.XR.ARFoundation`, and `UnityEngine.XR.Interaction.Toolkit.Inputs.Readers`.
    - It defines several serialized fields and properties, including `tapStartPositionInput`, `arPlaneManager`, `characterPrefab`, and `raycastManager`.
    - It also defines private fields to manage the character's state, such as `spawnedCharacter`, `targetPosition`, and various boolean flags.

2. **Awake Method**:
    - Checks if the `raycastManager` is attached and logs an error if it is missing.

3. **Update Method**:
    - Reads the tap input and updates the state flags.
    - Calls `HandleTap` if a tap is detected.
    - Calls `MoveCharacter` if the character is moving.

4. **HandleTap Method**:
    - Performs a raycast to detect AR planes at the tap position.
    - Spawns the character at the hit position if it is not already spawned.
    - Sets the target position for the character to move towards.

5. **MoveCharacter Method**:
    - Moves the character towards the target position using a `CharacterController`.
    - Rotates the character to face the target position.
    - Plays or stops the walking animation based on the character's movement.


Example Code:

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.Interaction.Toolkit.Inputs.Readers;

public class CharacterTapController : MonoBehaviour
{
     [SerializeField]
     XRInputValueReader<Vector2> m_TapStartPositionInput = new XRInputValueReader<Vector2>("Tap Start Position");

     public XRInputValueReader<Vector2> tapStartPositionInput
     {
          get => m_TapStartPositionInput;
          set => XRInputReaderUtility.SetInputProperty(ref m_TapStartPositionInput, value, this);
     }

     public ARPlaneManager arPlaneManager;
     public GameObject characterPrefab;
     public ARRaycastManager raycastManager;

     private GameObject spawnedCharacter;
     private Vector3 targetPosition;
     private bool isCharacterSpawned = false;
     private bool m_IsPerformed = false;
     private bool m_WasPerformedThisFrame = false;
     private bool m_WasCompletedThisFrame = false;
     private Vector2 m_TapStartPosition;
     private bool isMoving = false;

     void Awake()
     {
          if (raycastManager == null)
          {
                Debug.LogError("ARRaycastManager component is missing. Please attach it to the GameObject.");
          }
     }

     void Update()
     {
          var prevPerformed = m_IsPerformed;
          var prevTapStartPosition = m_TapStartPosition;
          var tapPerformedThisFrame = tapStartPositionInput.TryReadValue(out m_TapStartPosition) && prevTapStartPosition != m_TapStartPosition;

          m_IsPerformed = tapPerformedThisFrame;
          m_WasPerformedThisFrame = !prevPerformed && m_IsPerformed;
          m_WasCompletedThisFrame = prevPerformed && !m_IsPerformed;

          if (m_WasPerformedThisFrame)
          {
                HandleTap(m_TapStartPosition);
          }

          if (isMoving)
          {
                MoveCharacter();
          }
     }

     void HandleTap(Vector2 tapPosition)
     {
          List<ARRaycastHit> hits = new List<ARRaycastHit>();
          if (raycastManager.Raycast(tapPosition, hits, UnityEngine.XR.ARSubsystems.TrackableType.PlaneWithinPolygon))
          {
                Pose hitPose = hits[0].pose;

                if (!isCharacterSpawned)
                {
                     // Spawn the character on the first tap
                     spawnedCharacter = Instantiate(characterPrefab, hitPose.position, hitPose.rotation);
                     isCharacterSpawned = true;
                }
                
                // Set the target position and start moving the character
                targetPosition = hitPose.position;
                isMoving = true;
          }
          else
          {
                Debug.Log("No plane hit detected at tap position.");
          }
     }

     void MoveCharacter()
     {
          if (spawnedCharacter == null) return;

          // Move character towards target position
          CharacterController characterController = spawnedCharacter.GetComponent<CharacterController>();
          Animator animator = spawnedCharacter.GetComponent<Animator>();

          if (characterController != null && animator != null)
          {
                Vector3 direction = targetPosition - spawnedCharacter.transform.position;
                direction.y = 0; // Ignore Y axis for smooth horizontal movement

                if (direction.magnitude > 0.1f)
                {
                     Vector3 movement = direction.normalized * Time.deltaTime * 1.0f; // Adjust speed as needed
                     characterController.Move(movement);

                     // Rotate character to face target position
                     Quaternion targetRotation = Quaternion.LookRotation(direction);
                     spawnedCharacter.transform.rotation = Quaternion.Slerp(spawnedCharacter.transform.rotation, targetRotation, Time.deltaTime * 5f);

                     // Play walking animation
                     animator.SetBool("IsRunning", true);
                }
                else
                {
                     // Stop walking animation
                     animator.SetBool("IsRunning", false);
                     isMoving = false;
                }
          }
          else
          {
                Debug.LogError("Character does not have a CharacterController or Animator component.");
          }
     }
}
```
