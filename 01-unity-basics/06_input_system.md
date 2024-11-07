# Unity 3D New Input System

Unity's new Input System provides a more flexible and powerful way to handle input compared to the old Input Manager. Instead of using simple `Input.GetAxis` calls, the new system allows you to define input actions that can be easily managed and extended.

## Setting Up the New Input System

1. **Install the Input System Package**:
    - Open the Package Manager (`Window > Package Manager`).
    - Search for "Input System" and install it.

2. **Enable the New Input System**:
    - Go to `Edit > Project Settings > Player`.
    - Under `Other Settings`, change the `Active Input Handling` to `Both` or `Input System Package (New)`.

## Creating Input Actions

1. **Create an Input Actions Asset**:
    - Right-click in the Project window and select `Create > Input Actions`.
    - Name your new Input Actions asset (e.g., `PlayerInputActions`).

2. **Define Actions**:
    - Double-click the Input Actions asset to open the Input Actions editor.
    - Create a new action map (e.g., `Player`).
    - Add actions to the action map (e.g., `Move`, `Jump`, `Fire`).
    - Assign bindings to each action (e.g., `Move` can be bound to `WASD` keys or a gamepad stick).

## Using Input Actions in Code

1. **Generate a C# Class**:
    - In the Input Actions editor, click the `Generate C# Class` button.
    - This will create a C# script that you can use to access your input actions.

2. **Implementing Input in a Script**:
    - Create a new MonoBehaviour script (e.g., `PlayerController`).
    - Use the generated C# class to handle input actions.

```csharp
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerController : MonoBehaviour
{
     private PlayerInputActions inputActions;

     private void Awake()
     {
          inputActions = new PlayerInputActions();
     }

     private void OnEnable()
     {
          inputActions.Player.Enable();
          inputActions.Player.Move.performed += OnMove;
          inputActions.Player.Jump.performed += OnJump;
     }

     private void OnDisable()
     {
          inputActions.Player.Disable();
          inputActions.Player.Move.performed -= OnMove;
          inputActions.Player.Jump.performed -= OnJump;
     }

     private void OnMove(InputAction.CallbackContext context)
     {
          Vector2 movement = context.ReadValue<Vector2>();
          // Handle movement
     }

     private void OnJump(InputAction.CallbackContext context)
     {
          // Handle jump
     }
}
```

## Advantages of the New Input System

- **Flexibility**: Easily rebind controls and support multiple devices.
- **Modularity**: Define input actions in a centralized asset.
- **Extensibility**: Add new input actions without changing existing code.

By using the new Input System, you can create more robust and maintainable input handling for your Unity projects.