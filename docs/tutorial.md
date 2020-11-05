---
title: "Tutorial"
date: 2019-12-11T16:59:07+11:00
tags: []
summary: "Start working with Yarn Spinner."
toc: true
menu:
  docs:
    weight: 100   
---

**Welcome to Yarn Spinner!** In this tutorial, you'll learn how to use Yarn Spinner in a Unity project to create interactive dialogue. 

We'll start by downloading and installing Yarn Spinner. We'll then take a look at the core concepts that power Yarn Spinner, and write some dialogue. After that, we'll explore some of the more advanced features of Yarn Spinner.

## Introducing Yarn Spinner

Yarn Spinner is a tool for writing interactive dialogue in games - that is, conversations that the player can have with characters in the game. Yarn Spinner does this by letting you write your dialogue in a programming language called *Yarn*. 

Yarn is designed to be as minimal as possible. For example, the following is valid Yarn code:

```yarn
Gregg: I think I might be sick.
Mae: True friendship: Letting your friend make you sick.
Gregg: True bros.
Mae: True bros.
```

Yarn Spinner will take each line, one at a time, and deliver them to the game. It's entirely up to the game to decide what to do with the lines; for example, the game that these lines are from, [Night in the Woods](http://nightinthewoods.com), displays them in speech bubbles, one character at a time, and waits for the user to press a button before showing the next one.

## Quick Start

We'll start by creating an empty Unity project, installing Yarn Spinner, and then playing the example game. We'll then make some changes to the example game.

* **Unity 2018.4 users**: We recommend that you download the `.unitypackage` when reading this tutorial instead of using the Unity Package Manager, because you won't otherwise get the sample project. If you're using a later version of Unity, you can install it via the Package Manager or via a `.unitypackage`.

### Playing the example game

We'll begin by playing the example game that comes with Yarn Spinner. It's very short - about 2 minutes long.

1. **Create a new empty Unity project.** Choose the 2D template.
2. **Download and install Yarn Spinner.** Go to the [installation]({{< ref "docs/unity/installing.md" >}}) instructions, and follow the directions there.   
3. **Open the example scene.** 
   * If you installed Yarn Spinner via a `.unitypackage`, you'll find it in the `Yarn Spinner/Examples/Space` folder.
   * If you installed Yarn Spinner the Package Manager, import the Space sample, and then find the scene in the `Samples/Yarn Spinner/<version>/Space` folder.
4. **Play the game.** Use the left and right arrow keys to move, and the space bar to talk to characters.

We're now ready to start looking under the hood, to see how Yarn Spinner powers this game.

### The Yarn Editor

Yarn Spinner stores its dialogue in `.yarn` files. These are plain text files, which means you can edit them in any plain text editor ([Visual Studio Code](https://code.visualstudio.com) is a good option, and we offer a [syntax highlighting extension](https://marketplace.visualstudio.com/items?itemName=SecretLab.yarn-spinner) to make it nice to use!)

<!-- Link to VS Code Extension -->

You can also use the [Yarn Editor](https://yarnspinnertool.github.io/YarnEditor), which is a web-based tool for working with Yarn code. The Yarn Editor is useful, because it lets you view the structure of your dialogue in a very visual way.

<!-- Write a Yarn Editor tutorial and link it here probably  -->

## Reading Yarn

In this section of the tutorial, we're going to open the file `Sally.yarn`, and look at what it's doing.

1. **Open `Sally.yarn` in your editor of choice.**

Yarn Spinner groups all of its dialogue into **nodes**. Nodes contain everything: your lines of dialogue, the choices you show to the player, and the commands that you send to the game. The `Sally.yarn` file contains four of them:  `Sally`, `Sally.Watch`, `Sally.Exit`, and `Sally.Sorry`. The example game is set up so that when you walk up to Sally and press the spacebar, the game will start running the `Sally` node. 

2. **Go to the `Sally` node.**

Let's take a look at what that node contains. Here's the entire text of it:

```yarn
<<if visited("Sally") is false>>
    Player: Hey, Sally. #line:794945
    Sally: Oh! Hi. #line:2dc39b
    Sally: You snuck up on me. #line:34de2f
    Sally: Don't do that. #line:dcc2bc
<<else>>
    Player: Hey. #line:a8e70c
    Sally: Hi. #line:305cde
<<endif>>

<<if not visited("Sally.Watch")>>
    [[Anything exciting happen on your watch?|Sally.Watch]] #line:5d7a7c
<<endif>>

<<if $sally_warning and not visited("Sally.Sorry")>>
    [[Sorry about the console.|Sally.Sorry]] #line:0a7e39
<<endif>>
[[See you later.|Sally.Exit]] #line:0facf7
```

Take a second now to look at this code, and get a feel for its structure.

### Lines and Logic

We'll now take a closer look at each part of this code, and explain what's going on.

```yarn
<<if visited("Sally") is false>>
    Player: Hey, Sally. #line:794945
    Sally: Oh! Hi. #line:2dc39b
    Sally: You snuck up on me. #line:34de2f
    Sally: Don't do that. #line:dcc2bc
<<else>>
    Player: Hey. #line:a8e70c
    Sally: Hi. #line:305cde
<<endif>>
```

The first line of code in this node checks to see if Yarn Spinner has already run this node. `visited` is a function that this example game has defined - it isn't built into Yarn Spinner. It returns `true` if the node you specify has been run before. You'll notice that this line is wrapped in `<<` and `>>` symbols. This tells Yarn Spinner that it's control code, and not meant to be shown to the player.

<!-- add a tutorial on defining functions -->

If they haven't run the `Sally` node yet, it means that this is the first time that we've spoken to Sally in this game. As a result, we run lines in which Sally and the player character meet. Otherwise, we instead run some shorter lines.

Each line in Yarn Spinner is just a run of text, which is sent directly to the game. It's up to the game to decide how it wants to display it; in the example game, it's shown at the top of the screen.

At the end of each line, you'll see a `#line:` tag. This tag lets Yarn Spinner identify lines across multiple translations, and is optional if you aren't translating your game into other languages. Yarn Spinner can automatically generate them for you.

### Options

Here's the next part of the code.

```yarn
<<if not visited("Sally.Watch")>>
    [[Anything exciting happen on your watch?|Sally.Watch]] #line:5d7a7c
<<endif>>

<<if $sally_warning and not visited("Sally.Sorry")>>
    [[Sorry about the console.|Sally.Sorry]] #line:0a7e39
<<endif>>
```

In the next part of the code, we do a check, and if it passes, we add an *option*. Options are things that the player can select; in this game, they're things the player can say, but like lines, it's up to the game to decide what to do with them. Options are shown to the player when the end of a node is reached.

The first couple of lines here test to see whether the player has run the node `Sally.Watch`. If they haven't, then the code adds a new option. Options are wrapped with `[[` and `]]`. The text before the `|` is shown to the player, and the text after is the name of the node that will be run if the player chooses the option. Like lines, options can have line tags for localisation.

If the player *has* run the `Sally.Watch` node before, this code won't be run, which means that the option to run it again won't appear.

The rest of this part does a similar thing as the first: it does a check, and adds another option if the check passes. In this case, it checks to see if the variable `$sally_warning` is true, and if the player has not yet run the `Sally.Sorry` node. `$sally_warning` is set in a different node - it's in the node `Ship`, which is stored in the file `Ship.yarn`.

```yarn
[[See you later.|Sally.Exit]] #line:0facf7
```

The very last line of the node adds an option, which takes the player to the `Sally.Exit` line. Because this option isn't inside an `if` statement, it's always added.

When Yarn Spinner hits the end of the node, all of the options that have been accumulated so far will be shown to the player. Yarn Spinner will then wait for the player to make a selection, and then start running the node that they selected.

And that's how the node works!

## Writing Some Dialogue

Let's write some dialogue! We'll add a couple of lines to the Ship.

1. **Open the file `Ship.yarn`.** It contains a single node, called `Ship` - go to it.

This code uses couple of features that we didn't see in `Sally`: *commands*, and *variables*. 

### Commands

Commands are messages that Yarn Spinner sends to your game, but aren't intended to be shown to the player. Commands let you control things in your scene, like moving the camera around, or instructing a character to move to another point. 

Because every game is different, Yarn Spinner leaves the task of defining most commands to you. Yarn Spinner defines two built-in commands: `wait`, which pauses the dialogue for a certain number of seconds, and `stop`, which ends the dialogue immediately.

The example game defines its own command, `setsprite`, which is used to change the sprite that the Ship character's face is displaying. You can see this in action in the file `Ship.yarn`:

```yarn
Player: How's space?
Ship: Oh, man.
<<setsprite ShipFace happy>>
Ship: It's HUGE!
<<setsprite ShipFace neutral>>
```

{{<note>}}
You can learn how to define your own custom commands in {{<xref "/docs/unity/working-with-commands">}}.
{{</note>}}

### Variables

Variables are how you store information about what the player has done in the game. We saw variables in use in the `Sally` node, where the variable `$sally_warning` was used to control whether some content was shown or not. This variable is set in here, in the `Ship` node - it represents whether or not the player has heard Sally's warning about the console from the Ship.

Variables in Yarn Spinner start with a `$`, and can store text, numbers, booleans (true or false values), or `null`. If you try and access a variable that hasn't been set, you'll get the value `null`, which represents "no value".

### Adding Some Content

2. **Add some new dialogue.** Add the following text to the end of the node:

```yarn

Ship: Anything else I can help with?

-> No, thanks.
    Ship: Aw, ok!
-> I'm good.
    Ship: Let me know!

Ship: Bye!
```

### Shortcut Options

The `->` items that we just added are called **shortcut options**. Shortcut options let you put choices in your node without having to create new nodes, which you link to through the `[[Option]]` syntax. They exist in-line with the rest of your node.

To use a shortcut option, you write a `->`, followed by the text that you want to display. Then, on the next lines, indent the code a few spaces (it doesn't matter how many, as long as you're consistent.) The indented lines will run if the option they're attached to is selected. Shortcut options can be nested, which means you can put a group of shortcut options inside another. You can put any kind of code inside a shortcut option's lines.

Because shortcut options don't require you to create new nodes, they're really good for situations where you want to offer the player some kind of choice that doesn't significantly change the flow of the story.

3. **Save the file, and go back to the game.** Play the game again, and talk to the Ship. At the end of the conversation, you'll see new dialogue.

## Where Next

The example game is set up so that when you talk to Sally, the node `Sally` is run, and when you talk to the Ship, the node `Ship` is run. With this in mind, change the story so that after you get told off by Sally, she asks you to go and fix a problem with the Ship.

You can also read the [Syntax Reference]({{< ref "syntax.md" >}}) for Yarn Spinner.


<!-- 
Where to go from here:

* [Customising the UI](ref "customising-ui.md")
* [Creating Yarn Commands](ref "creating-yarn-commands.md")
-->
