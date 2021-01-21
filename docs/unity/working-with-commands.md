---
title: "Working With Commands"
date: 2019-12-14T17:11:10+11:00
tags: []
summary: "Learn how to access and create commands in Unity."
draft: false
toc: true
menu: 
    docs:
        title: "Commands"
        parent: "unity"
        weight: 3
  
---

In Yarn Spinner, you can send instructions to your game through **commands**. Commands look like this:

```yarn
<<wait 2>>
<<setsprite ShipName happy>>
<<fade_out 1.5>>
```

Commands are sent to your game's Dialogue Runner, just like lines and options are. Commands are generally not directly shown to the player; instead, they're used for things like stage directions.

## Built-in Commands

There are two built-in commands in Yarn Spinner: `wait`, and `stop`.

### `wait`

The `wait` command pauses the dialogue for a specified number of seconds, and then resumes. You can use integers (whole numbers), or decimals.

```yarn
// Wait for 2 seconds
<<wait 2>>

// Wait for half a second
<<wait 0.5>>
```

### `stop`

The `stop` command immediately ends the dialogue, as though the game had reached the end of a node. Use this if you need to leave a conversation in the middle of an `if` statement, or a shortcut option.

```yarn
// Leave the dialogue now
<<stop>>

// Leave the dialogue if we don't have enough money
<<if $money < 50>>
    Shopkeeper: You can't afford my pies!
    <<stop>>
<<endif>>
```

## Defining New Commands

You can define your own commands, which allow the scripts you write in Yarn Spinner to control parts of the game that you've built.

In Unity, there are two ways to add new commands to Yarn Spinner: automatically, via the `YarnCommand` attribute, or manually, using the `DialogueRunner`'s `AddCommandHandler` method.

### The `YarnCommand` attribute

The `YarnCommand` attribute lets you expose methods in a `MonoBehaviour` to Yarn Spinner. 

When you add the `YarnCommand` attribute to a method, you specify what name the command should have in Yarn scripts. You can then use that name, along with the name of the game object that should receive that command, as a command.

For example:

```csharp
public class CharacterMovement : MonoBehaviour {

    [YarnCommand("jump")]
    public void Jump() {
        Debug.Log("Jumping!");
    }
}
```

If you save this in a file called `CharacterMovement.cs`, create a new game object called `MyCharacter`, and attach the `CharacterMovement` script to that game object, you can run this code in your Yarn scripts like this:

```yarn
<<jump MyCharacter>>
// will print "Jumping!" in the console
```

You can also use methods that take parameters. For example, consider this method:

```csharp

[YarnCommand("walk")]
public void Walk(string destinationName) {
    // figure out the position of the object 
    // 'destinationName' and start walking to it
}
```

This command could be called like this:

```yarn
<<walk MyCharacter StageLeft>> // walk to the position of the object named 'StageLeft'
```

You must provide as many parameters as the method expects to receive. Methods with optional parameters are supported; if you don't provide a value for an optional parameter in your Yarn script, the parameter's default value will be used, just like when you call the method from C#.

#### Supported Parameter Types

Parameters are allowed to be any of the following types:

* `string`
* `integer`
* `double`
* `float`
* `bool`
* Any other type that implements the [IConvertible](https://docs.microsoft.com/en-us/dotnet/api/system.iconvertible) interface
* `GameObject`
* `Component`

If a parameter is a `GameObject`, Yarn Spinner will search the scene for an object whose name matches the parameter you provided. If one isn't found, the value `null` will be passed.

If a parameter is a `Component` (or any of its subclasses, like `Transform` or `Rigidbody`), Yarn Spinner will search the scene for an object with the given name, and then look for an object of the type of the parameter. If an object with the right name can't be found, or if it can but it doesn't have the right component, the value `null` will be passed.


{{<note>}}
If a parameter is any other supported type, it will be converted from a `string` using the [`Convert.ChangeType`](https://docs.microsoft.com/en-us/dotnet/api/system.convert.changetype#System_Convert_ChangeType_System_Object_System_Type_System_IFormatProvider_) method. The invariant culture will be used as the format provider to perform the conversion.
{{</note>}}

#### Using Coroutines

You can make the Dialogue Runner wait for your commands to finish doing something before continuing. For example, you might want to have a command that makes a character walk to a point, and the Dialogue Runner should wait until that's done.

If the method you provide is a [`Coroutine`](https://docs.unity3d.com/Manual/Coroutines.html), the Dialogue Runner will wait for the coroutine to finish before continuing.

### Adding commands through code

You can also add new commands directly to a Dialogue Runner, using the `AddCommandHandler` method.

When you call `AddCommandHandler`, you provide two parameters the name of the command, and a [delegate](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates/) (that is, a reference to a method, or an in-line function) to run when the command is called.

The delegate can take up to 6 parameters. These parameters can be any of the [supported parameter types](#supported-parameter-types).

For example, to create a command that makes the main camera look at an object, create a new C# script in Unity with the following code:

```csharp
public class CustomCommands : MonoBehaviour {    

    // Drag and drop your Dialogue Runner into this variable.
    public DialogueRunner dialogueRunner;

    public void Awake() {

        // Create a new command called 'camera_look', which looks
        // at a target.
        dialogueRunner.AddCommandHandler(
            "camera_look",
            (GameObject target) => {
                // Make the main camera look at this target
                Camera.main.transform.LookAt(target.transform);

                // The dialogue runner will continue immediately
                // after returning from this method                
            }             
        );
    }
}
```

Add this script to any game object, and it will register the `camera_look` in the Dialogue Runner you attach.

You can then call this method like this:

```yarn
<<camera_look LeftMarker>> // make the camera look at an object named LeftMarker
```

#### Using Coroutines

Just as with commands you register using the `YarnCommand` attribute, you can make the Dialogue Runner wait for your command to finish by using coroutines.

* If the delegate you provide returns `void`, the Dialogue Runner will continue running your code as soon as the delegate returns.
* If the delegate returns a [`Coroutine`](https://docs.unity3d.com/Manual/Coroutines.html), the Dialogue Runner will wait until that coroutine finishes, and then contintue running your code.

For example, here's how you'd write your own custom implementation of `<<wait>>`. (You don't have to do this in your own games, because `<<wait>>` is already added for you, but this example shows you how you'd do it yourself.)

```csharp
public class CustomWaitCommand : MonoBehaviour {    

    // Drag and drop your Dialogue Runner into this variable.
    public DialogueRunner dialogueRunner;

    public void Awake() {

        // Create a new command called 'custom_wait', which 
        // waits for one second.
        dialogueRunner.AddCommandHandler(
            "custom_wait",
            (float duration) => { 
                return StartCoroutine(CustomWait(duration))
            }
        );
    }

    private IEnumerator CustomWait() {

        // Wait for 1 second
        yield return new WaitForSeconds(1.0);
        
        // Yarn Spinner will now continue running!
    }
}
```

This new method, once registered with a Dialogue Runner, can be called like this:

```yarn
<<custom_wait>> // Waits for one second, then continues running
```

{{<note>}}
Note that you must *return* the coroutine - that is, you must return the value that `StartCoroutine` returns. If you just call `StartCoroutine` and don't return its value, the coroutine will be started, but the Dialogue Runner won't wait for it to finish.
{{</note>}}

#### Using Methods

In addition to using delegates, you can also provide a method to the `AddCommandHandler` method. 

There are a couple of reasons you might want to do this: if you use a method rather than a delegate, your command can have optional parameters. Additionally, it can keep your code a little tidier, because you won't have code nested inside function calls.

If you use a method, and that method takes parameters, you'll need to specify their types when calling `AddCommandHandler`. (You don't need to do this when you use a delegate.)

Methods can return `void`, or a `Coroutine`, just like command delegates can.

For example, here's a command that takes two parameters: a string, and an optional integer:

```csharp
public class CustomMethodCommand : MonoBehaviour {    

    // Drag and drop your Dialogue Runner into this variable.
    public DialogueRunner dialogueRunner;

    public void Awake() {

        // Create a new command called 'add_string_and_int', 
        // which logs its two parameters:
        dialogueRunner.AddCommandHandler<string, int>(
            "add_string_and_int",
            AddStringAndInteger
        );
    }

    private void AddStringAndInteger(string str, int i = 5) {
        Debug.Log($"String: {str}, int: {i}");        
    }
}
```

This command can be run from your Yarn script like this:

```yarn
<<add_string_and_int hello 1>> // logs "String: hello, int 1"
<<add_string_and_int bye>> // logs "String: bye, int 5"
```

## Functions

As we've detailed above, Commands can only return `void` or a `Coroutine`, and must be called one at a time per line. But what if you want to return a `bool`, `string`, or `float` from C# and use it in an ``<<if>>``?

[**Functions**]({{<ref "functions.md">}}) let you perform operations and return data back to the Yarn script. For example, if we define a `random()` function in C# and add it to the Dialogue Runner, then we can call that function to generate a random number from 1-100 in Yarn:

```yarn
// random() is a C# function that generates a random number from 1-100
<<if random() > 50>>
    You win the coin flip!
<<else>>
    You lost the coin flip!
<<endif>>
```

For more on defining and implementing Functions, see [Functions]({{<ref "functions.md">}}).