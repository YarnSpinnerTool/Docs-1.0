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

The Variable Storage is responsible for storing the values of Yarn variables (see {{<xref "/docs/writing/expressions-and-variables">}}). 

Yarn Spinner is designed to work alongside an existing data storage system, so ideally you customize your own variable storage, or integrate with your chosen third-party solution.

## InMemoryVariableStorage

Yarn Spinner comes with a very simple in-memory variable storage system, which keeps everything in memory and doesn't automatically save anything to disk by default. If you want to write your own variable storage system, it can be a useful reference.

{{<img "/docs/unity/img/v2.0/variable-storage-inspector.png">}}

* **Debug Text View**: Optional. Lists all variables and current values with a Unity UI [Text](https://docs.unity3d.com/Packages/com.unity.ugui@1.0/manual/script-Text.html). Good for debug builds.
* **Debug Variables**: Lists all variables and current values, but in the inspector itself. When debugging in editor, this might be faster or more convenient than Debug Text View.

{{<note>}}
As of v2.0, the old **Default Variables** functionality has been moved to "Declarations" in the {{<xref "/docs/unity/yarn-programs">}} asset.
{{</note>}}

## Saving / loading / serializing variable data

**Serialization** is the process of converting program state into data, while **deserialization** is the process of reconstructing program state from data. In games, we often use serialization for "save file" functionality. 

As of v2.0, the default `InMemoryVariableStorage` stores variable types (`bool`, `string`, `float`) as `System.Object` types in a dictionary. This can be a bit tricky to serialize, so as an example we've implemented some basic serialization functions like `SerializeAllVariablesToJSON()` and `DeserializeAllVariablesFromJSON()` using Unity's built-in [JSONUtility](https://docs.unity3d.com/Manual/JSONSerialization.html). Some example JSON data:

```json
{
   "keys":[
      "System.String/$stringVar",
      "System.Boolean/$boolVar",
      "System.Single/$floatVar"
   ],
   "values":[
      "hola",
      "True",
      "1.42"
   ]
}
```

We also have two example save / load implementations that wrap around the serialization functions: `LoadFromFile()` and `SaveToFile()` read / write .json files (we recommend using Unity's [`Application.persistentDataPath`](https://docs.unity3d.com/ScriptReference/Application-persistentDataPath.html) as part of the filepath), while `LoadFromPlayerPrefs()` and `SaveToPlayerPrefs()` use Unity's built-in [PlayerPrefs](https://docs.unity3d.com/ScriptReference/PlayerPrefs.html) system.

## Create a variable storage from scratch

To create your own variable storage from scratch, implement the `VariableStorageBehaviour` abstract class, itself a subclass of `MonoBehaviour`.

1. Create a new C# script, and call it (for example) `CustomVariableStorage.cs`.

2. Next, change the parent class from `MonoBehaviour` to `Yarn.Unity.VariableStorageBehaviour`.

3. Finally, add the following methods to it. You will need to add your own behaviour:

```csharp

public class CustomVariableStorage : Yarn.Unity.VariableStorageBehaviour {

    // main function used by Dialogue Runner to retrieve Yarn variable values
    public override bool TryGetValue<T>(string variableName, out T result) {
        // retrieve value with key "variableName" from a list or dictionary, etc.
    }
    
    // overload for setting a String variable
    public override void SetValue(string variableName, string stringValue) {
        // save "stringValue" under key "variableName" to a dictionary, etc.
    }
    
    // overload for setting a Float variable
    public override void SetValue(string variableName, float floatValue) {
        // save to a dictionary, etc.
    }
    
    // overload for setting a Boolean variable
    public override void SetValue(string variableName, bool boolValue) {
        // save to a dictionary, etc.
    }

    // clear all variable data        
    public override void Clear() {
        // clear all your dictionaries
    }
}
```

{{<note>}}
As of v2.0, `VariableStorageBehaviour` no longer uses `Yarn.Value`, to help enforce static typing in Yarn scripts.
{{</note>}}

4. You're now able to add this script to your Dialogue Runner object. Don't forget to set the Dialogue Runner's Variable Storage field to the component that you added.
