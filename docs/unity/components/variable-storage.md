---
title: "Variable Storage"
date: 
tags: []
draft: false
toc: true
weight: 3
menu: 
    docs:
        parent: "components"
---

The Variable Storage is responsible for storing the values of variables. Yarn Spinner is designed to work alongside an existing data storage system, so it doesn't handle things like saving your game. Instead, it provides a mechanism for writing your own variable storage system, so that you can handle saved games yourself, or integrate with a third-party solution.

## `InMemoryVariableStorage`

Yarn Spinner does come with a very simple in-memory variable storage system, which keeps everything in memory and doesn't save anything to disk. If you want to write your own variable storage system, it can be a useful reference.

{{<img "/docs/unity/img/v1.1/memory-storage-inspector.png">}}

* **Default Variables**: A list of initial values to set when the scene loads.
  * When you specify the initial value, don't include the `$` at the start of the variable name.
* **Debug Text View**: A Unity UI [Text](https://docs.unity3d.com/Packages/com.unity.ugui@1.0/manual/script-Text.html) object. If this value is not empty, its text will be set to the current value of all variables.

## Creating your own variable storage

To create your own variable storage, you need to implement the `VariableStorageBehaviour` abstract class. This class is a subclass of `MonoBehaviour`, which means that instances of it can be attached to game objects.

To create your own, follow these steps:

1. Create a new C# script, and call it (for example) `CustomVariableStorage.cs`.

2. Next, change the parent class from `MonoBehaviour` to `Yarn.Unity.VariableStorageBehaviour`.

3. Finally, add the following methods to it. You will need to add your own behaviour :

```csharp

public class CustomVariableStorage : Yarn.Unity.VariableStorageBehaviour {
        

    // Store a value into a variable
    public virtual void SetValue(string variableName, Yarn.Value value) {
        // 'variableName' is the name of the variable that 'value' 
        // should be stored in.
    }

    // Return a value, given a variable name
    public virtual Yarn.Value GetValue(string variableName) {
        // 'variableName' is the name of the variable to return a value for
    }

    // Return to the original state
    public virtual void ResetToDefaults () {

    }
}
```

{{<note>}}
`VariableStorageBehaviour`'s methods deal in terms of `Yarn.Value`s. These objects are containers for strings, numbers, boolean values, or null.
{{</note>}}

4. You're now able to add this script to your Dialogue Runner object. Don't forget to set the Dialogue Runner's Variable Storage field to the component that you added.
