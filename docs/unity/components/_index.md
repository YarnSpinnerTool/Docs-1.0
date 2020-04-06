---
title: "Yarn Spinner for Unity Components"
date: 
tags: []
summary: "Learn about the components used in Yarn Spinner for Unity."
draft: false
toc: true
hide_contents: true
weight: 1
menu: 
    docs:
        parent: "unity"
        weight: 2
        title: "Components"
        identifier: "components"
---

Yarn Spinner for Unity is made up of several components, and they each do a different task. In this document, you'll learn what the different pieces do, how they work together, and how they work with your game.

## [Yarn Programs]({{<ref "/docs/unity/components/yarn-programs" >}})

When you import a `.yarn` file into Unity, Yarn Spinner for Unity will read its contents, compile the Yarn script, and convert it into a Yarn Program. A Yarn Program is the compiled representation of the program, and is what ships with your game when you create a build. 

To learn more about Yarn Programs, see {{<xref "/docs/unity/components/yarn-programs">}}.

## Dialogue Runner

The *Dialogue Runner* takes Yarn Programs, and runs their contents. The Dialogue Runner is a Unity component that's attached to a game object in your scene, and communicates with two other important components - the [Dialogue UI](#dialogue-ui), and the [Variable Storage](#variable-storage). 

To learn more about the Dialogue Runner, see {{<xref "/docs/unity/components/dialogue-runner">}}.

## Dialogue UI

The *Dialogue UI* is responsible for delivering the lines, options and commands from the Dialogue Runner to the user. The Dialogue UI is also responsible for telling the Dialogue Runner when to start and stop its execution, so that the dialogue can wait for a line to be displayed, or for an option to be selected.

To learn more about the Dialogue UI, see {{<xref "/docs/unity/components/dialogue-ui">}}.

## Variable Storage

The *Variable Storage* is a component that acts as the bridge between Yarn Spinner's variables and your game's storage. Yarn Spinner doesn't store variable data itself; instead, it talks to the Variable Storage, which decides how to store the data. This means that you can integreate Yarn Spinner's data into a larger saved-game system.

To learn more about the Variable Storage, see {{<xref "/docs/unity/components/variable-storage">}}.