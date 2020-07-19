---
title: "Voice Over Playback"
date: 
tags: []
draft: false
toc: true
weight: 3
menu: 
    docs:
        parent: "components"
---

The Voice Over Playback is responsible for playing back the voice over audio clip of the current dialogue line. Yarn Spinner comes with two different components for this task depending on the audio engine you use. 

## Voice Over Playback Unity

{{<img "/docs/unity/img/v2.0/voice-over-component.png.png">}}

You'll need to drag the component into the DialogueRunner's `Dialogue Views` array so that the `Voice Over Playback Unity` receives updates about the current dialogue line. Apart from that, no settings need to be changed for simple voice over playback.  

You can change the following fields to adjust the voice over playback behaviour to your needs:

* **Fade Out Time On Line Finish**: If a line is being skipped or a new line is interrupting the current line, the currently playing voice over audio clip will be faded out to prevent clicking artifacts. You can specify the duration of the fade out in seconds.
* **Audio Source**: The audio source that should be used to playback the current voice over audio clip. Useful if you want to add custom audio filters or tweak certain audio source parameters of your voice over audio clip. If this field is empty, an audio source with default settings will be used for playback.

## Voice Over Playback FMOD

!!CREATE INFO ABOUT FMOD SUPPORT!!