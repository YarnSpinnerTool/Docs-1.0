---
title: "Yarn Programs"
date: 
tags: []
draft: false
toc: true
weight: 1
menu: 
    docs:
        parent: "components"        
---

A Yarn Program is the compiled form of your `.yarn` script. Yarn Spinner for Unity converts the Yarn script into this asset when the file is imported, and it's used by the {{<xref "/docs/unity/components/dialogue-runner">}} to run your dialogue.

{{<img "/docs/unity/img/v1.1/yarn-program-assets.png" "A Yarn Program, with a string table inside" >}}

## String Tables

{{<note>}}
It is recommended to set up your {{<xref "/docs/unity/components/project-settings">}} language list first to make the language selection on the Yarn inspector easier by showing only the languages relevant to your project.
{{</note>}}

As part of the compilation process, the Yarn Spinner compiler separates your Yarn script into two pieces: the code, and the *string table*. The string table contains the content that the player actually sees, like the lines an options; the code contains the instructions on what to show and when. The Yarn Program is an on-disk asset that contains the code, as well as the reference to the string table.

String tables are `TextAssets`. They contain all of the lines that were found in the script, stored in comma-separated values format.

An imported Yarn Program always contains at least one string table. You can choose to create more, in order to support different languages besides the one you wrote the dialogue in.

## Localisations

String tables are always associated with a language. This string table's language is known as the *base language*, because it's generated from the original text of your file.

When you first import a `.yarn` file, the string table's base language is set to your current language on your computer or to the first language from the {{<xref "/docs/unity/components/project-settings">}} language list. The name of the string table will include its language code. For example, if your computer's language is set to Australian English or Australian English is the first language in your {{<xref "/docs/unity/components/project-settings">}} language list, the base localisation will be set to `en-AU`.

{{<note>}}
To learn more about localisation and translation, see {{<xref "/docs/unity/localisation">}}.
{{</note>}}

### Changing the base language

You can change the base language for a Yarn Program. To do this, select the file in the Assets pane, and go to the Inspector. From there, open the Base Language menu, and choose your language.

{{<img "/docs/unity/img/v1.1/yarn-program-change-base-language.png" "The Base Language menu in the Inspector." >}}

### Adding more localisations

To add more languages besides the base language, open the languagues menu *below* Base Language, and select the language you want to add. Next, click the Create New Localisation button.

{{<img "/docs/unity/img/v1.1/yarn-program-add-language.png" "Adding a new language in the Inspector." >}}

When you do this, a new `TextAsset` will be created on disk. The name of the new file will include the language code that you selected.

{{<img "/docs/unity/img/v1.1/yarn-program-add-language-asset.png" "A newly generated string table." >}}

The new string table contains a copy of the base language; once you have it, you can rewrite the lines to your desired language. The Yarn Program will keep a reference to the new string table, and associate it with the language that you selected. This allows the {{<xref "/docs/unity/components/dialogue-runner">}} to load the right lines based on the player's language. 

You can see the list of localisations that a Yarn Program has in the "Localizations" list in the Inspector.

{{<img "/docs/unity/img/v1.1/yarn-program-add-language-inspector.png" "The list of languages for a Yarn Program." >}}

## Voice Overs

You can add voice over audio clips for every line in your Yarn asset. The list is filtered by the language selected in the dropdown menu right above the first line. The content of that dropdown menu is defined by the audio languages that have been added to YarnSpinner's project languages list.
{{<img "/docs/unity/img/v2.0/voice-overs-inspector.png" "The Voice Overs list in the Yarn inspector." >}}

On the left side, you have the linetag and the text of that line in the language selected in the dropdown menu. Use the part right to that to link this line with an AudioClip.

The system also supports Addressables. You'll first need to enable it in Yarn Spinner's project settings window. (ADD REFERENCE)

You can also click on the `Import Voice Over Audio Files` button to automatically link every linetag with it's coresponding audio clip for all languages. This will search for files that have the line's linetag in their filename and are located in a directory with the name of the language key (like `en-AU` for Australian English).

{{<img "/docs/unity/img/v2.0/voice-overs-import.png" "The recommended voice over file and directory structure." >}}

It is also possible to include the language key in the filename and put the files into a directory with any name but this is not recommended. For example, the linetag `34de2f` will collide with the language key `de` when you include German in your project language list.
