# Using VS Code with Unity

Visual Studio Code (VS Code) is a popular code editor that can be used for Unity development. Below are the steps and configurations needed to set up VS Code with Unity.

## Prerequisites

1. **Unity Hub and Unity Editor**: Ensure you have the latest version of Unity installed.
2. **Visual Studio Code**: Download and install VS Code from [here](https://code.visualstudio.com/).

## Configurations and Extensions

### Unity Project

1. **Open Unity Project**: Open your Unity project in the Unity Editor.
2. **Set External Script Editor**:
    - Go to `Edit > Preferences > External Tools`.
    - Set `External Script Editor` to `Visual Studio Code`.

### Visual Studio Code

1. **Install Extensions**:
    - **C#**: Provides C# language support.
    - **Unity Tools**: Adds support for Unity snippets and commands.
    - **Debugger for Unity**: Allows debugging of Unity scripts.
    - **Unity Code Snippets**: Provides useful code snippets for Unity development.

2. **Configure OmniSharp**:
    - Open VS Code and go to `File > Preferences > Settings`.
    - Search for `OmniSharp: Use Global Mono` and set it to `always`.

3. **Install .NET SDK**:
    - Download and install the .NET SDK from [here](https://dotnet.microsoft.com/download).

## Additional Packages

1. **Visual Studio Code Editor Package**:
    - In Unity, go to `Window > Package Manager`.
    - Search for `Visual Studio Code Editor` and install it.

## Using VS Code with Unity

1. **Open Scripts**:
    - Double-click any C# script in Unity to open it in VS Code.
    
2. **Debugging**:
    - Set breakpoints in your code within VS Code.
    - Attach the debugger by selecting `Run > Start Debugging` or pressing `F5`.

By following these steps, you can effectively use VS Code for Unity development, enhancing your productivity and coding experience.