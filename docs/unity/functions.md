---
title: "Yarn Functions"
date: 2021-01-20T17:11:10+11:00
tags: []
summary: "Learn how to access and create Yarn Functions in Unity."
draft: false
toc: true
menu: 
    docs:
        title: "Functions"
        parent: "unity"
        weight: 4
  
---

When running Yarn scripts in Unity, you can send instructions to your game and/or call C# code in two ways:

- [Commands]({{<ref "working-with-commands.md">}}) are like stage directions sent to Unity via the [Dialogue Runner]({{ref "dialogue-runner.md">}}), but they are generally one-way. You *cannot* use Commands to return data back to the Yarn script. 
- However, **Functions** in Yarn scripts *can* call C# code via the Dialogue Runner *and* `return` data back to the Yarn script.

## Defining new Functions

To use a function in a Yarn script:

1. Write C# code for the function.
2. Add the function to the Dialogue Runner with `DialogueRunner.AddFunction()`. This forms the link between C# and Yarn.
3. You can now call the function in the Yarn script.

Functions can return any of the following C# types readable as Yarn variables:
* `bool`
* `string`
* `float`

## Function example: random()

What if you wanted to generate a random number in Yarn? With a function, you can just use Unity's random number generator and then return the result to the Yarn script.

First we create a new C# script called `RandomNumberInYarn.cs` and put it on the same game object as the Dialogue Runner. 

Then we define our function in C#:

```csharp
public class RandomNumberInYarn : MonoBehaviour {
    
    public float GetRandomValue() {
        return Random.Range(0, 100); // return a random number between 0 and 99 (will never return 100)
    }
}
```

And in the same script's `Start()` method, notify the Dialogue Runner with `DialogueRunner.AddFunction()` and a [lambda expression](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-expressions):

```csharp
    void Start() {
        // registers a function "random()" in Yarn
        GetComponent<DialogueRunner>().AddFunction("random", () => {return GetRandomValue();} );
        // note how the C# func name ("GetRandomValue") can be different from the Yarn func name ("random")
    }
```

Finally, call `random()` in a Yarn script:

```yarn
Here's a random number: { random() }
You flip a coin...
<<if random() < 50 >> // 50% chance
    ... and the coin is heads!
<<else>>
    ... and the coin is tails!
<<endif>>
```

## Function example: visited()

The Space sample included with Yarn Spinner implements a node tracking system made of two parts: (1) keeping a list of which Yarn nodes have been visited, and (2) a **function** that returns `true` if the particular Yarn node has been visited already.

First we create `NodeVisitedTracker.cs` and put it on the same game object as the Dialogue Runner. 

In the script, we initialize an empty list of all the nodes we've visited already. Then in the Dialogue Runner inspector we configure it to call `NodeComplete()` each time we finish a node, and add that node's name to the list.

```csharp
public class NodeVisitedTracker : MonoBehaviour {

    // a list of all the Yarn node names we have visited
    List<string> _visitedNodes = new List<string>(); // this could be a HashSet if we want more efficient search

    // this is called via DialogueRunner.OnNodeComplete (a UnityEvent assigned in the inspector)
    // which is fire every time the player leaves a Yarn node "nodeName"
    public void NodeComplete(string nodeName) {
        _visitedNodes.Add(nodename);
    }
}
```

Let's add more to the script. Given a node name, `WasVisited()` tells us whether that node name is in our visited list already, returning `true` or `false`.

```csharp
    // the function we will call from the Yarn script
    public bool WasVisited(string nodeName) {
        return _visitedNodes.Contains(nodeName);
    }
```

Then in the script's `Start()` function, we register the script with the Dialogue Runner and a lambda expression.

```csharp
    public DialogueRunner dialogueRunner; // assign in inspector

    void Start() {
        // registers a function "visited()" in Yarn
        dialogueRunner.AddFunction("visited", (string nodeName) => {return WasVisited(nodeName);} );
        // note how the C# func name ("WasVisited") can be different from the Yarn func name ("visited")
    }
```

Or instead of declaring `WasVisited()` and using a lambda expression, we could also define and register the function all at once with an [anonymous method](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/anonymous-functions) and `delegate`:

```csharp
    // an alternate way of defining + registering the function "visited()"
    dialogueRunner.AddFunction("visited", 
        delegate(string nodeName) {
            return _visitedNodes.Contains(nodeName);
        }
    );
}
```

Either way, the Dialogue Runner knows about our function and we can now use `visited()` in a Yarn script:

```yarn
title: Sally
---
<<if visited("Sally") is false>>
    // the first time we talk to Sally
    Player: Hey, Sally.
    Sally: Oh! Hi.
<<else>>
    // the second time we talk to Sally
    Sally: I remember you!
    Player: Hello again.
<<endif>>
===
```

### Command vs. Functions

Both Commands and Functions provide ways to interface with Unity and call C# methods from a Yarn script. They might seem similar, but they actually behave very differently. To summarize:

Commands
- like a program instruction; one per line
- can interrupt or pause a Yarn script (e.g. `<<stop>>`, `<<wait>>` or [`Coroutine`](https://docs.unity3d.com/Manual/Coroutines.html) )
- takes only input and does not return output; *cannot* return data back to the Yarn script

Functions
- like a program operator; executed in-line
- cannot pause or interrupt Yarn scripts, always executes within a frame
- takes input and returns output; *can* `return` values back to the Yarn script

Given these tradeoffs, **we recommend using Commands most of the time**, because being able to pause or pace your dialogue script is important for storytelling and encourages good scripting habits.

However, if you have a specific need to process an input and do something with the resulting output, then use a Function.