# State Machines in Unity 3D

## Fundamental Concepts

### State Machines
A state machine is a computational model used to design algorithms. It consists of a finite number of states, transitions between these states, and actions. State machines are particularly useful in game development for managing complex behaviors in a structured and predictable manner.

### States
A state represents a specific condition or situation of an object. In Unity, states can be used to define different behaviors of game objects, such as idle, walking, running, or jumping. Each state encapsulates the logic that should be executed when the object is in that particular state.

### Transitions
Transitions define how and when an object moves from one state to another. They are triggered by specific conditions or events, such as user input, collisions, or timers. Transitions ensure that the state machine can change states in response to the game's dynamics.

### Actions
Actions are the operations that are performed when entering, exiting, or during a state. These can include animations, sound effects, or any other game logic. Actions help to define the behavior of the game object in each state.

### Benefits of Using State Machines
- **Organization**: State machines provide a clear and organized way to manage different behaviors and transitions.
- **Maintainability**: They make it easier to update and maintain game logic by isolating state-specific code.
- **Predictability**: State machines ensure predictable behavior by clearly defining state transitions and actions.

State machines are a powerful tool in Unity 3D for controlling game object behaviors, making it easier to manage complex interactions and ensuring a smooth gameplay experience.


## Example of Game Manager with State Machine for The Treasure Collector

In this section, we will create a GameManager that uses a state machine to manage the game state. The player has 30 seconds to collect 3 items in the scene. Each time an item is collected, a sound is played.

### Game States
1. **Start**: The game is ready to start.
2. **Playing**: The player is actively collecting items.
3. **GameOver**: The game ends when the player runs out of time or collects all items.

### GameManager Implementation

```csharp
using UnityEngine;
using TMPro;

public class GameManager : MonoBehaviour
{
    private enum State
    {
        Start,
        Playing,
        GameOver
    }

    private State currentState;
    private int itemsCollected;
    private float timeRemaining = 30f;
    public TextMeshProUGUI timerText;
    public TextMeshProUGUI itemsCollectedText;
    public AudioSource collectSound;

    void Start()
    {
        currentState = State.Start;
        itemsCollected = 0;
        UpdateUI();
    }

    void Update()
    {
        switch (currentState)
        {
            case State.Start:
                HandleStartState();
                break;
            case State.Playing:
                HandlePlayingState();
                break;
            case State.GameOver:
                HandleGameOverState();
                break;
        }
    }

    private void HandleStartState()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            currentState = State.Playing;
        }
    }

    private void HandlePlayingState()
    {
        timeRemaining -= Time.deltaTime;
        if (timeRemaining <= 0 || itemsCollected >= 3)
        {
            currentState = State.GameOver;
        }
        UpdateUI();
    }

    private void HandleGameOverState()
    {
        Debug.Log("Game Over");
    }

    private void UpdateUI()
    {
        timerText.text = "Time: " + Mathf.Max(0, Mathf.FloorToInt(timeRemaining)).ToString();
        itemsCollectedText.text = "Items Collected: " + itemsCollected.ToString();
    }

    private void OnTriggerEnter(Collider other)
    {
        if (other.CompareTag("Collectible") && currentState == State.Playing)
        {
            itemsCollected++;
            collectSound.Play();
            Destroy(other.gameObject);
        }
    }
}
```


This GameManager script manages the game states and transitions between them based on the player's actions and game events. It ensures that the player has a limited time to collect items, and plays a sound each time an item is collected.


## Example: Controlling Game States in "Collector of Treasures"

In this example, we will create a state machine to control the game states of "Collector of Treasures," where the player fights against monsters and collects treasures.

### Game States
1. **Idle**: The player is standing still without any actions.
2. **Walking**: The player is moving around the map.
3. **Collecting**: The player collects coins, triggering a sound effect.
4. **Fighting**: The player engages in combat with monsters.
5. **Using Special Skill**: The player uses a special skill associated with their current weapon.
6. **Game Over**: The player is defeated by monsters.

### State Machine Implementation

```csharp
using UnityEngine;

public class PlayerStateMachine : MonoBehaviour
{
    private enum State
    {
        Idle,
        Walking,
        Collecting,
        Fighting,
        UsingSpecialSkill,
        GameOver
    }

    private State currentState;

    void Start()
    {
        currentState = State.Idle;
    }

    void Update()
    {
        switch (currentState)
        {
            case State.Idle:
                HandleIdleState();
                break;
            case State.Walking:
                HandleWalkingState();
                break;
            case State.Collecting:
                HandleCollectingState();
                break;
            case State.Fighting:
                HandleFightingState();
                break;
            case State.UsingSpecialSkill:
                HandleUsingSpecialSkillState();
                break;
            case State.GameOver:
                HandleGameOverState();
                break;
        }
    }

    private void HandleIdleState()
    {
        // Logic for idle state
        if (Input.GetKeyDown(KeyCode.W))
        {
            currentState = State.Walking;
        }
    }

    private void HandleWalkingState()
    {
        // Logic for walking state
        if (Input.GetKeyDown(KeyCode.Space))
        {
            currentState = State.Collecting;
        }
        else if (Input.GetKeyDown(KeyCode.F))
        {
            currentState = State.Fighting;
        }
    }

    private void HandleCollectingState()
    {
        // Logic for collecting state
        CollectCoin();
        currentState = State.Idle;
    }

    private void HandleFightingState()
    {
        // Logic for fighting state
        if (Input.GetKeyDown(KeyCode.S))
        {
            currentState = State.UsingSpecialSkill;
        }
        else if (Input.GetKeyDown(KeyCode.G))
        {
            currentState = State.GameOver;
        }
    }

    private void HandleUsingSpecialSkillState()
    {
        // Logic for using special skill state
        UseSpecialSkill();
        currentState = State.Fighting;
    }

    private void HandleGameOverState()
    {
        // Logic for game over state
        Debug.Log("Game Over");
    }

    private void CollectCoin()
    {
        // Play sound effect and collect coin logic
        Debug.Log("Coin Collected");
    }

    private void UseSpecialSkill()
    {
        // Use special skill logic
        Debug.Log("Special Skill Used");
    }
}
```

This state machine manages the player's different states and transitions between them based on user input and game events. It ensures that the player's actions are organized and predictable, enhancing the gameplay experience.

### Adding More Complexity

To make the state machine more complex, we can add additional states, transitions, and actions. For example, we can introduce states for "Jumping," "Attacking," and "Defending," and add more detailed logic for each state.

### Additional Game States
1. **Jumping**: The player is in the air after jumping.
2. **Attacking**: The player performs an attack move.
3. **Defending**: The player defends against an attack.

### Updated State Machine Implementation

```csharp
using UnityEngine;

public class PlayerStateMachine : MonoBehaviour
{
    private enum State
    {
        Idle,
        Walking,
        Collecting,
        Fighting,
        UsingSpecialSkill,
        Jumping,
        Attacking,
        Defending,
        GameOver
    }

    private State currentState;

    void Start()
    {
        currentState = State.Idle;
    }

    void Update()
    {
        switch (currentState)
        {
            case State.Idle:
                HandleIdleState();
                break;
            case State.Walking:
                HandleWalkingState();
                break;
            case State.Collecting:
                HandleCollectingState();
                break;
            case State.Fighting:
                HandleFightingState();
                break;
            case State.UsingSpecialSkill:
                HandleUsingSpecialSkillState();
                break;
            case State.Jumping:
                HandleJumpingState();
                break;
            case State.Attacking:
                HandleAttackingState();
                break;
            case State.Defending:
                HandleDefendingState();
                break;
            case State.GameOver:
                HandleGameOverState();
                break;
        }
    }

    private void HandleIdleState()
    {
        // Logic for idle state
        if (Input.GetKeyDown(KeyCode.W))
        {
            currentState = State.Walking;
        }
        else if (Input.GetKeyDown(KeyCode.Space))
        {
            currentState = State.Jumping;
        }
    }

    private void HandleWalkingState()
    {
        // Logic for walking state
        if (Input.GetKeyDown(KeyCode.Space))
        {
            currentState = State.Collecting;
        }
        else if (Input.GetKeyDown(KeyCode.F))
        {
            currentState = State.Fighting;
        }
        else if (Input.GetKeyDown(KeyCode.J))
        {
            currentState = State.Jumping;
        }
    }

    private void HandleCollectingState()
    {
        // Logic for collecting state
        CollectCoin();
        currentState = State.Idle;
    }

    private void HandleFightingState()
    {
        // Logic for fighting state
        if (Input.GetKeyDown(KeyCode.S))
        {
            currentState = State.UsingSpecialSkill;
        }
        else if (Input.GetKeyDown(KeyCode.A))
        {
            currentState = State.Attacking;
        }
        else if (Input.GetKeyDown(KeyCode.D))
        {
            currentState = State.Defending;
        }
        else if (Input.GetKeyDown(KeyCode.G))
        {
            currentState = State.GameOver;
        }
    }

    private void HandleUsingSpecialSkillState()
    {
        // Logic for using special skill state
        UseSpecialSkill();
        currentState = State.Fighting;
    }

    private void HandleJumpingState()
    {
        // Logic for jumping state
        if (IsGrounded())
        {
            currentState = State.Idle;
        }
    }

    private void HandleAttackingState()
    {
        // Logic for attacking state
        PerformAttack();
        currentState = State.Fighting;
    }

    private void HandleDefendingState()
    {
        // Logic for defending state
        PerformDefense();
        currentState = State.Fighting;
    }

    private void HandleGameOverState()
    {
        // Logic for game over state
        Debug.Log("Game Over");
    }

    private void CollectCoin()
    {
        // Play sound effect and collect coin logic
        Debug.Log("Coin Collected");
    }

    private void UseSpecialSkill()
    {
        // Use special skill logic
        Debug.Log("Special Skill Used");
    }

    private void PerformAttack()
    {
        // Perform attack logic
        Debug.Log("Attack Performed");
    }

    private void PerformDefense()
    {
        // Perform defense logic
        Debug.Log("Defense Performed");
    }

    private bool IsGrounded()
    {
        // Check if the player is grounded
        return true; // Simplified for example purposes
    }
}
```

This updated state machine includes additional states and transitions, making it more complex and detailed. It provides a more comprehensive control over the player's actions and interactions within the game.


## Example: Quiz Game with State Machine

In this example, we will create a state machine to control a quiz game using UI TextMeshPro buttons. The quiz will consist of a list of questions and answers provided in serialized objects. The answers will be randomized when displayed.

### Game States
1. **Start**: The quiz is ready to start.
2. **Question**: A question is displayed, and the player has 30 seconds to answer.
3. **Answer**: The correct answer is shown for 5 seconds.
4. **GameOver**: The quiz ends, and the player's score is displayed.

### QuizManager Implementation

```csharp
using UnityEngine;
using TMPro;
using UnityEngine.UI;
using System.Collections.Generic;

public class QuizManager : MonoBehaviour
{
    [System.Serializable]
    public struct Question
    {
        public string questionText;
        public string[] answers;
    }

    public List<Question> questions;
    public TextMeshProUGUI questionText;
    public TextMeshProUGUI questionNumberText;
    public TextMeshProUGUI correctAnswersText;
    public Button[] answerButtons;
    public TextMeshProUGUI timerText;

    private enum State
    {
        Start,
        Question,
        Answer,
        GameOver
    }

    private State currentState;
    private int currentQuestionIndex;
    private int correctAnswers;
    private float timeRemaining;
    private bool isAnswerCorrect;

    void Start()
    {
        currentState = State.Start;
        currentQuestionIndex = 0;
        correctAnswers = 0;
        UpdateUI();
    }

    void Update()
    {
        switch (currentState)
        {
            case State.Start:
                HandleStartState();
                break;
            case State.Question:
                HandleQuestionState();
                break;
            case State.Answer:
                HandleAnswerState();
                break;
            case State.GameOver:
                HandleGameOverState();
                break;
        }
    }

    private void HandleStartState()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            currentState = State.Question;
            DisplayQuestion();
        }
    }

    private void HandleQuestionState()
    {
        timeRemaining -= Time.deltaTime;
        if (timeRemaining <= 0)
        {
            currentState = State.Answer;
            isAnswerCorrect = false;
            DisplayAnswer();
        }
        UpdateUI();
    }

    private void HandleAnswerState()
    {
        timeRemaining -= Time.deltaTime;
        if (timeRemaining <= 0)
        {
            currentQuestionIndex++;
            if (currentQuestionIndex >= questions.Count)
            {
                currentState = State.GameOver;
            }
            else
            {
                currentState = State.Question;
                DisplayQuestion();
            }
        }
        UpdateUI();
    }

    private void HandleGameOverState()
    {
        questionText.text = "Quiz Over! You answered " + correctAnswers + " questions correctly.";
    }

    private void DisplayQuestion()
    {
        timeRemaining = 30f;
        Question question = questions[currentQuestionIndex];
        questionText.text = question.questionText;
        List<string> answers = new List<string>(question.answers);
        answers.Shuffle();
        for (int i = 0; i < answerButtons.Length; i++)
        {
            answerButtons[i].GetComponentInChildren<TextMeshProUGUI>().text = answers[i];
            answerButtons[i].onClick.RemoveAllListeners();
            string selectedAnswer = answers[i];
            answerButtons[i].onClick.AddListener(() => OnAnswerSelected(selectedAnswer == question.answers[0]));
        }
        UpdateUI();
    }

    private void DisplayAnswer()
    {
        timeRemaining = 5f;
        questionText.text = isAnswerCorrect ? "Correct!" : "Wrong! The correct answer was: " + questions[currentQuestionIndex].answers[0];
        UpdateUI();
    }

    private void OnAnswerSelected(bool isCorrect)
    {
        isAnswerCorrect = isCorrect;
        if (isCorrect)
        {
            correctAnswers++;
        }
        currentState = State.Answer;
        DisplayAnswer();
    }

    private void UpdateUI()
    {
        questionNumberText.text = "Question " + (currentQuestionIndex + 1) + " / " + questions.Count;
        correctAnswersText.text = "Correct Answers: " + correctAnswers;
        timerText.text = "Time: " + Mathf.Max(0, Mathf.FloorToInt(timeRemaining)).ToString();
    }
}

public static class ListExtensions
{
    private static System.Random rng = new System.Random();

    public static void Shuffle<T>(this IList<T> list)
    {
        int n = list.Count;
        while (n > 1)
        {
            n--;
            int k = rng.Next(n + 1);
            T value = list[k];
            list[k] = list[n];
            list[n] = value;
        }
    }
}
```

This QuizManager script manages the quiz states and transitions between them based on the player's actions and game events. It ensures that the player has a limited time to answer each question, displays the correct answer, and tracks the number of correct answers.