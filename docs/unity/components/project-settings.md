---
title: "Project Settings"
date: 
tags: []
draft: false
toc: true
weight: 1
menu: 
    docs:
        parent: "components"        
---
This window allows you to define project-wide settings that Yarn Spinner uses like the available text or audio languages in your project. It is completely optional but makes the use of Yarn Spinner easier. You can open the project settings window via `Edit->Project Settings->Yarn Spinner`.

## Project Languages

{{<img "/docs/unity/img/v2.0/project-settings.png" "Yarn Spinner's project settings window" >}}

You can add a new language to your project by selecting it from the dropdown list and clicking on `Add language to project`. This will add the language to the list below. A language added to this list will automatically be available to the project as `Text Language`. The first language in that list is the project's default language, often referred to as `Base Language` (the language in which your writers write Yarn files). You can still manually overwrite this per Yarn file but any newly imported Yarn file will get the first language in that list assigned as Base Language from now on (instead of the language of your OS). All language settings available on the inspector of a Yarn file are now reduced to the list of languages from the settings window.

All languages in that list have a checkbox to their right indicating if that language also has voice overs.

Settings made in this part are written to your ProjectSettings folder of your Unity project to the file `YarnProjectSettings.json`. The `ProjectSettings` class can be used to retrieve that list which can be useful for instance when creating option menus.

## Language Preferences

As soon as you have added languages to your `Project Languages` list, you can define a text and audio language that you prefer to use for your editor session. You can change these settings in play mode and they'll be applied as soon as the DialogueRunner starts the next line.

These settings will be stored in your PlayerPrefs. When creating an option menu for your project, it is recommended to read from and write to exactly these keys from the PlayerPrefs.

There is a main menu in Yarn Spinner's example files demonstrating how to set this up.
