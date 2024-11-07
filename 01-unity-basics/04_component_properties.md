# Unity Component Properties

## Using Enum for Defined Items

To add a property with defined items (like a combobox) to your Unity script, you can use an `enum`. Enums allow you to define a set of named values, which can then be used as a property in your script. These values will appear as a dropdown (combobox) in the Unity Editor.

### Example

```csharp
using UnityEngine;

public class Monster : MonoBehaviour
{
    public enum MonsterType
    {
        Goblin,
        Troll,
        Dragon,
        Vampire
    }

    [SerializeField]
    private MonsterType currentType = MonsterType.Goblin;

    void Update()
    {
        switch (currentType)
        {
            case MonsterType.Goblin:
                // Handle Goblin type
                break;
            case MonsterType.Troll:
                // Handle Troll type
                break;
            case MonsterType.Dragon:
                // Handle Dragon type
                break;
            case MonsterType.Vampire:
                // Handle Vampire type
                break;
        }
    }
}
```

In this example, the `MonsterType` enum defines four possible types for the monster: `Goblin`, `Troll`, `Dragon`, and `Vampire`. The `currentType` property uses this enum and will appear as a dropdown in the Unity Editor, allowing you to select one of the defined types.

By using enums, you can create properties with predefined values, making it easier to manage and configure your components in the Unity Editor.

### Playing Different Sounds Based on Enum

You can extend the previous example to play different sounds when the monster attacks or dies based on the `MonsterType` enum. To do this, you can use the `AudioSource` component in Unity.

### Example

```csharp
using UnityEngine;

public class Monster : MonoBehaviour
{
    public enum MonsterType
    {
        Goblin,
        Troll,
        Dragon,
        Vampire
    }

    [SerializeField]
    private MonsterType currentType = MonsterType.Goblin;

    [SerializeField]
    private AudioClip goblinAttackSound;
    [SerializeField]
    private AudioClip trollAttackSound;
    [SerializeField]
    private AudioClip dragonAttackSound;
    [SerializeField]
    private AudioClip vampireAttackSound;

    [SerializeField]
    private AudioClip goblinDieSound;
    [SerializeField]
    private AudioClip trollDieSound;
    [SerializeField]
    private AudioClip dragonDieSound;
    [SerializeField]
    private AudioClip vampireDieSound;

    private AudioSource audioSource;

    void Start()
    {
        audioSource = GetComponent<AudioSource>();
    }

    void Update()
    {
        // Example of playing attack sound
        if (Input.GetKeyDown(KeyCode.Space))
        {
            PlayAttackSound();
        }

        // Example of playing die sound
        if (Input.GetKeyDown(KeyCode.D))
        {
            PlayDieSound();
        }
    }

    void PlayAttackSound()
    {
        switch (currentType)
        {
            case MonsterType.Goblin:
                audioSource.PlayOneShot(goblinAttackSound);
                break;
            case MonsterType.Troll:
                audioSource.PlayOneShot(trollAttackSound);
                break;
            case MonsterType.Dragon:
                audioSource.PlayOneShot(dragonAttackSound);
                break;
            case MonsterType.Vampire:
                audioSource.PlayOneShot(vampireAttackSound);
                break;
        }
    }

    void PlayDieSound()
    {
        switch (currentType)
        {
            case MonsterType.Goblin:
                audioSource.PlayOneShot(goblinDieSound);
                break;
            case MonsterType.Troll:
                audioSource.PlayOneShot(trollDieSound);
                break;
            case MonsterType.Dragon:
                audioSource.PlayOneShot(dragonDieSound);
                break;
            case MonsterType.Vampire:
                audioSource.PlayOneShot(vampireDieSound);
                break;
        }
    }
}
```

In this example, the `Monster` class has additional `AudioClip` fields for attack and die sounds for each monster type. The `PlayAttackSound` and `PlayDieSound` methods play the corresponding sound based on the `currentType` enum value. The sounds are played when the spacebar or 'D' key is pressed, respectively.

By integrating enums with audio clips, you can easily manage and play different sounds for various monster types in your Unity game.



## Unity Attributes to Classes

Unity provides several attributes that you can use to enhance the properties in your MonoBehaviour scripts. These attributes help you organize your components, provide additional information, and improve the usability of your scripts in the Unity Editor.

#### [AddComponentMenu()]

The `[AddComponentMenu()]` attribute allows you to specify the menu path where your script will appear when adding it as a component in the Unity Editor. This can help you organize your scripts into specific categories, making them easier to find.

```csharp
using UnityEngine;

[AddComponentMenu("Custom/Monster")]
public class Monster : MonoBehaviour
{
    // Your script code here
}
```

In this example, the `Monster` script will appear under the "Custom" category in the "Add Component" menu.

#### [HelpURL()]

The `[HelpURL()]` attribute allows you to specify a URL that provides additional information or documentation about your script. This URL will be accessible from the Unity Editor, making it easier for other developers to find relevant information.

```csharp
using UnityEngine;

[HelpURL("https://example.com/monster-docs")]
public class Monster : MonoBehaviour
{
    // Your script code here
}
```

In this example, a help icon will appear next to the `Monster` script in the Unity Editor, linking to the specified URL.

## [Tooltip]

The `[Tooltip()]` attribute allows you to add a tooltip to a field in your script. This tooltip will appear when you hover over the field in the Unity Editor, providing additional information about the property.

```csharp
using UnityEngine;

public class Monster : MonoBehaviour
{
    [Tooltip("The type of monster.")]
    public MonsterType currentType;

    // Your script code here
}
```

In this example, when you hover over the `currentType` field in the Unity Editor, the tooltip "The type of monster." will be displayed.

By using these attributes, you can improve the organization, documentation, and usability of your MonoBehaviour scripts in the Unity Editor.

## Using Range for Defined Values

To restrict a numeric property to a specific range of values in your Unity script, you can use the `[Range]` attribute. This attribute allows you to define a minimum and maximum value for a property, which will be enforced in the Unity Editor.

### Example

```csharp
using UnityEngine;

public class Monster : MonoBehaviour
{
    [Range(-100, 200)]
    public int health;

    void Update()
    {
        // Example usage of the health property
        if (health < 0)
        {
            Debug.Log("Monster is dead.");
        }
        else if (health > 0)
        {
            Debug.Log("Monster is alive with health: " + health);
        }
    }
}
```

In this example, the `health` property is restricted to values between -100 and 200. In the Unity Editor, a slider will be displayed for this property, allowing you to select a value within the defined range.

By using the `[Range]` attribute, you can ensure that numeric properties in your scripts are constrained to specific values, making it easier to manage and configure your components in the Unity Editor.
