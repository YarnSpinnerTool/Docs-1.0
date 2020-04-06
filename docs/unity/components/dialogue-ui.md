---
title: "Dialogue UI"
date: 
tags: []
draft: false
toc: true
weight: 3
menu: 
    docs:
        parent: "components"
---

The Dialogue UI is responsible for delivering content to the player, receiving option selections, and generally acting as the bridge between the Dialogue Runner and the player.

## The Inspector

{{<img "/docs/unity/img/v1.1/dialogue-ui-inspector.png">}}

* **Dialogue Container**: The game object that should be made active when dialogue is active. The object will be made inactive when dialogue ends.
* **Text Speed**: The amount of time, in seconds, to wait before displaying the next character in a line. If this is set to zero, all of the text is displayed at once.
* **Option Buttons**: A list of Unity UI [Buttons](https://docs.unity3d.com/Packages/com.unity.ugui@1.0/manual/script-Button.html) that will be used to display options to the player.
  * Buttons will be made active or inactive depending on whether the dialogue is currently awaiting a selection, and depending on the number of options that are being presented.
  * The Dialogue UI expects each button to have a [`Text`](https://docs.unity3d.com/Packages/com.unity.ugui@1.0/manual/script-Text.html) or a TextMeshPro [`TMP_Text`](http://digitalnativestudios.com/textmeshpro/docs/textmeshpro-component/) as a child object. The text in the button will be set to the text of the relevant option.
  * You don't need to set an On Click handler for the buttons - the Dialogue UI will take care of this for you.
* **On Dialogue Start**: A Unity Event that will be called when dialogue starts.
  * Use this to do things like change your camera, disble gameplay UI, and other tasks you might need to do when dialogue begins.
* **On Dialogue End**: A Unity Event that will be called when dialogue ends.
* **On Line Start**: A Unity Event that will be called when a line is about to start being displayed.
* **On Line Finish Displaying**: A Unity Event that will be called when the line has finished being presented; that is, all of the text is now on the screen.
* **On Line Update**: A Unity Event that is called each time the Dialogue UI wishes to update the text that is to be displayed on the screen.
  * This event receives a parameter: the current text for the line. You can use this to send the current text to a Text UI element.
* **On Line End**: A Unity Event that is called when a Line has finished, and is no longer on screen.
* **On Options Start**: A Unity Event that is called when Options have been displayed on screen.
  * When this is called, the buttons in the Option Buttons list have been prepared, and are active.
* **On Options End**: A Unity Event that is when Options have been dismissed from the screen.
* **On Command**: A Unity Event that is called when a command has not been handled by any of the registered [command handlers]({{<ref "working-with-commands.md">}}).
  * This event receives a parameter: the text of the command that was received.