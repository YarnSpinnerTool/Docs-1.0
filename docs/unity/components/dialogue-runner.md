---
title: "Dialogue Runner"
date: 
tags: []
draft: false
toc: true
weight: 1
menu: 
    docs:
        parent: "components"
        weight: 2        
---

The Dialogue Runner is responsible for loading Yarn Programs, running their contents, and communicating with the {{<xref "/docs/unity/components/dialogue-ui">}}. The Dialogue Runner is also responsible for managing the loaded string tables and voice over assets. It uses the `ProjectSettings` class to retrieve the language preferences for the text and audio language to load the correct string table and voice over assets. You can set these in Yarn Spinner's {{<xref "/docs/unity/components/project-settings">}} `Language Preferences` section.

## The Inspector

{{<img "/docs/unity/img/v1.1/dialogue-runner-inspector.png" >}}

* **Yarn Scripts**: A list of [Yarn Programs]({{<ref "yarn-programs.md">}}). The nodes in these files will be loaded when the scene starts.
  * If this field is left blank, the [base language]({{<ref "yarn-programs.md#localisations">}}) is used.
* **Variable Storage**: The [variable storage object]({{<ref "variable-storage.md" >}}) that this dialogue runner will use to store and retrieve values from.
* **Dialogue Views**: The views that are used to present the content of the dialogue to the user. For example, the [dialogue UI]({{<ref "dialogue-ui.md">}}) will present the text of the dialogue on the screen. ADD VOICE OVER VIEW HERE. You can create your own view classes by inheriting from the class `DialogueViewBase` API LINK HERE.
* **Start Node**: The name of the node that will be run when the `StartDialogue` method is run without a parameter, or if **Start Automatically** is turned on.
  * If this field is blank, it will default to `Start`.
* **Start Automatically**: If this is turned on, the node specified in `Start Node` will start running.
  * If there's nothing specified in the Yarn Scripts field, then an error will be thrown.
* **Automatic Commands**: If this is turned on, the Dialogue Runner will look for methods that have [the `YarnCommand` attribute]({{<ref "working-with-commands.md">}}) to handle commands.
* **On Node Complete**: A Unity Event that will be called when the end of a node is reached.
  * Note that this may be called multiple times before the entire dialogue is complete, if there are multiple nodes that are run.
  
## Loading Yarn Programs with Code

When the Dialogue Runner starts, it will load all of the Yarn Programs specified in the Yarn Scripts field. If you want to provide a Yarn Program via code, you can use the the `Add` method to load them.

The following code demonstrates how to use `Add` to load a Yarn Program.

```csharp
public class NPC : MonoBehaviour {

    // The Yarn Program we want to load
    public YarnProgram scriptToLoad;

    // The dialogue runner we want to load the program into
    public DialogueRunner dialogueRunner;

    void Start () {
        // Load the program, along with all of its nodes. 
        // The string table will be selected based on the 
        // Dialogue Runner's text language variable.
        dialogueRunner.Add(scriptToLoad);                
    }
}
```

