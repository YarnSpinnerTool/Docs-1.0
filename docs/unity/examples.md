---
title: "Example Projects"
date: 
tags: []
summary: "Various examples, templates, and sample code for using Yarn Spinner with Unity."
draft: false
toc: true
weight: 5
menu: 
    docs:
        parent: "unity"
        title: "Examples"
---

Yarn Spinner for Unity comes with several sample projects that demonstrate functionality. 

Just open Unity, import Yarn Spinner, and check the `Samples` folder! 

(If you're using Unity Package Manager, you must click a button in the Package Manager window to manually import each sample.)

## Included Samples

**These samples were built as examples to teach and demo specific functionality and best practices, and they aren't complete enough to function as full project templates for entire games.** Again, we want to stress that Yarn Spinner isn't a complete game-making toolkit! It's just a lightweight framework for handling game dialogue. You still have to build the rest of your game.

{{< img "v2.0.0/SpaceSample.gif" "3D speech bubble example" >}}

`Space` is a basic 2D sidescroller that implements a basic player controller with keyboard input, NPC interaction, voice over. It also demos how to manage multiple Yarn scripts across multiple characters and have them interact.

{{< img "v2.0.0/YarnSpinner3dExample.gif" "3D speech bubble example" >}}

`3D` shows how to dynamically position speech bubbles above characters in the game world, much like in the top-down 3D game A Short Hike. It probably works in 2D with minimal changes, with results similar to Night In The Woods.

{{< img "v2.0.0/PhoneChatExample.gif" "phone chat app example" >}}

`PhoneChat` uses a second script to clone new message bubbles, which mimics a text message styled phone app. It also uses Yarn Commands to set the current speaker, an alternate way to define who's talking.

{{< img "v2.0.0/VisualNovelSample.gif" "visual novel sample" >}}

`VisualNovel` features extensive use of Yarn Commands to implement a surprisingly versatile Ren'py inspired framework for making visual novels. Even if you're not making a visual novel, you might learn something from the implementation. Its custom commands are documented on this page below.

## Visual Novel documentation ##

Importing character sprites: for reference, the sample character sprites are about 600 x 1200 pixels. Because sprites are instantiated automatically, the default sprite import settings matter a lot. Some suggested importer settings:

- Texture Type: Sprite
- Pixels Per Unit: 165 (try different numbers until the size feels right)
- Pivot: Bottom

---

### `<<Scene (spriteName)>>`
set background image to (spriteName)

---

### `<<Draw (spriteName), (x=0.5), (y=0.5)>>`
show (spriteName) at (x,y) in normalized screenspace (0.0-1.0)
- 0.0 is like 0% left / bottom of screen, 1.0 is like 100% right / top of screen
- for x or y, you can also use keywords like "right", "upper", "top" (0.75) or "middle", "center" (0.5) or "left", "lower", "bottom" (0.25)
- when x and/or y are omitted (e.g. `<<Show TreeSprite>>`) then they default to values of 0.5... thus, omitting both x and y would set the position to (0.5, 0.5) in normalized screenspace (center of the screen)

---

### `<<Hide (actorOrSpriteName)>>`
hide and delete any actor called (actorOrSpriteName) or any sprites called (actorOrSpriteName)
- this command supports wildcards (*)
    - example 1: `<<Hide Sally*>>` will hide all actors and sprites with names that start with "Sally"
    - example 2: `<<Hide *Cat>>` will hide all objects with names that end with "Cat"

---

### `<<HideAll>>`
hides all sprites and actors, takes no parameters

---

### `<<Act (actorName), (spriteName), (x=0.5), (y=0.5), (HTMLcolor=black) >>`
very useful for persistent characters... kind of like `<<Show>>` except it also has (actorName), which links the sprite to that actorName
- anytime a character named (actorName) talks in a Yarn script, this sprite will automatically highlight
- also good for changing expressions, e.g. calling `<<Act Dog, dog_happy, left>>` and then `<<Act Dog, dog_sad>>` will automatically swap the sprite and preserve its positioning... this is better than tediously calling `<<Show dog_happy>>` and then `<<Hide dog_happy>>` and then `<<Show dog_sad>>`, etc.
- (HTMLcolor) is a web hex color (like "#ffffff") or a common color keyword (like "white")... here, it affects the color of the name label in VNDialogueUI, but I suppose you could use it for anything

---

### `<<Flip (actorOrSpriteName), (xPosition=0.0)>>`
horizontally flips an actor or sprite, in case you need to make your characters look around or whatever
- if (xPosition) is defined, it will force the actor to face a specific direction

---

### `<<Move (actorOrSpriteName), (xPosition=0.5), (yPosition=0.5), (moveTime=1.0)>>`
animates / slides an actor or sprite toward a new position, good for animating walking or running
- (moveTime) is how long it will take to move from current position to new position, in seconds
- for notes on (xPosition) and (yPosition), see entry for `<<Show>>` above
- KNOWN BUG: not recommended to use this at the same time as a `<<Shake>>` call... let the previous call finish, first

---

### `<<Shake (actorOrSpriteName), (shakeTime=0.5)>>`
shakes an actor or a sprite, good for when someone is surprised or angry or laughing
- if shakeTime isn't specified, the default value 0.5 means "shake this object at 50% strength for 0.5 seconds"
- a shakeTime=1.0 means "shake this object at 100% for 1.0 seconds", and so on
- KNOWN BUG: not recommended to use this at the same time as a `<<Move>>` call... let the previous call finish, first

---

### `<<Fade (HTMLcolor=black), (startOpacity=0.0), (endOpacity=1.0), (fadeTime=1.0)>>`
fades the screen to a certain color, could be a fade in or a fade out depending on how you use it
- startOpacity and endOpacity are numbers 0.0-1.0 (like 0%-100%)
- by default with no parameters (`<<Fade>>`) will fade to black over 1.0 seconds
- (HTMLcolor) is a web hex color (like "#ffffff") or a common color keyword (like "white")

---

### `<<FadeIn (fadeTime=1.0)>>`
useful shortcut for fading in, no matter what the previous fade settings were... equivalent to calling something like `<<Fade black, -1, 0.0, 1.0>>` (where startOpacity=-1 means it will reuse the last fade command's colors and opacity)

---

### `<<CamOffset (xOffset=0), (yOffset=0), (moveTime=0.25)>>`
pans the camera based on x/y offset from the center over "moveTime" seconds
- example: ``<<CamOffset 0, 0, 0.1>>` centers the camera (default position) over 0.1 seconds
- note that the camera doesn't actually move, but fakes a camera movement by moving sprites
- by default, the background image is fixed like a skybox; if you want the background to pan too, then parent it to the "Sprites" game object in the prefab

---

### `<<PlayAudio (audioClipName),(volume),("loop")>>`
plays an audio clip named (audioClipName) at volume (0.0-1.0)
- if "loop" is present, it will loop the audio repeatedly until stopped; otherwise, the sound will end automatically

---

### `<<StopAudio (audioClipName)>>`
stop playing any active audio clip named (audioClipName)

---

### `<<StopAudioAll>>`
stops playing all sounds, no parameters

---

### Visual Novel asset credits
- Visual Novel Tutorial Set (public domain) https://opengameart.org/content/visual-novel-tutorial-set
- Lovely Piano Song by Rafael Krux (public domain) https://freepd.com
- Comic Game Loop - Mischief by Kevin MacLeod (public domain) https://freepd.com
