---
title: "Localisation and Translation"
date: 
tags: []
summary: "Learn how to translate your dialogue into multiple languages."
draft: false
toc: true
weight: 5
menu: 
    docs:
        parent: "unity"
        title: "Localising"
---

When you write dialogue in Yarn Spinner, you write text in a single language. However, when you want your game to be playable by people who don't speak that language, you need to be able to *localise* the dialogue. 

Localisation is the process of preparing your content so that it can be translated, and so that translated versions of the content can be used by the game.

The majority of this preparation work is done by defining new languages for your [Yarn Programs]({{<ref "yarn-programs.md">}}). By default, all Yarn Programs generate a *string table*, which is set to the *base language* of the Yarn Program.

## Line tags and string tables

The string table associates the text of each of the lines in the Yarn Program, as it was originally written, to an internal identifier called the *line tag*. A line tag is never shown to the player; instead, it serves as a way to refer to the same line across multiple translations.

If a line doesn't have a line tag defined in the `.yarn` file, the compiler will automatically generate a one. However, this tag is not guaranteed to be the same over time. If you're translating your dialogue into multiple languages, each line needs to have a line tag written at the end.

Line tags start with a `#line:`, and then end with a unique line identifier. They look like this:

```yarn
Lori: Why did you do it? #line:ee7d34
Mae: I don’t know. #line:08d17b
Lori: Did he have it coming? #line:c28e9b
Mae: No. #line:a6ad66
```

In this way, Yarn Spinner is able to uniquely identify line `a6ad66` with the text `Mae: No.`. The same line tag is used across all string tables for a Yarn Program, which means that the code is able to remain the same - that is, the same line identifiers are used in the same order across all languages, and only the text in the string table needs to change.

### Generating line tags

You can manually write the line tags yourself, but it's more efficient to get Yarn Spinner for Unity to add them for you. When Yarn Spinner detects that not all lines in a `.yarn` file have line tags, it won't let you create new localisations. Instead, it will offer to generate line tags for any line or option that is missing one.

{{<img "/docs/unity/img/v1.1/yarn-program-add-line-tags.png" "Automatic line tag adding in the Inspector">}}

When you use this button to add new line tags, they'll be added to the end of lines, options and shortcut options that don't already have them. If a line already has one, it will be left alone.

Once you've done this, you can begin creating new string tables, as discussed in {{<xref "/docs/unity/components/yarn-programs">}}.

### String tables

When you have generated a string table for another language, it will be created as a `.csv` file on disk. This is a *comma-separated values* file that you can open in a spreadsheet tool.

An example of this is shown in Figure 2:

<figure>

| id          | text                         | file | node           | lineNumber | 
|-------------|------------------------------|------|----------------|------------| 
| line:ee7d34 | Lori: Why did you do it?     | Lori | LoriIntroConvo | 29         | 
| line:08d17b | Mae: I don’t know.           | Lori | LoriIntroConvo | 30         | 
| line:c28e9b | Lori: Did he have it coming? | Lori | LoriIntroConvo | 31         | 
| line:a6ad66 | Mae: No.                     | Lori | LoriIntroConvo | 32         | 

<figcaption>Part of the string table generated from the above code.</figcaption>

</figure>

Once you have the generated `.csv`, you can translate the `text` column of each of the lines as needed. When the [Dialogue Runner]({{<ref "dialogue-runner.md#inspector">}})'s Text Language property is set to the language used by that string table, the corresponding text will be used.

## Localising inline expressions

Lines can contain [inline expressions]({{<ref "syntax.md#inline-expressions">}}), which allow values to be embedded inside lines. When a line is added to a string table, any inline expression is replaced with a placeholder.

For example, a line in a `.yarn` file that looks like this:

```yarn
You have {$gold} gold pieces. If you had twice as much, you'd have {$gold * 2}.
```

Will appear like this in the string table:

```
You have {0} gold pieces. If you had twice as much, you'd have {1}.
```

When translating a line into a different language, you can move or re-order the placeholders however you need. You can also remove them from the line entirely, if needed.

## Localising format functions

{{<note>}}
This feature has been deprecated, and is replaced with markup.
{{</note>}}

[Format functions]({{<ref "syntax.md#format-functions">}}) allow Yarn Spinner to replace text in a line, based on the value of a variable. 

When generating a string table, format functions are preserved in their entirety. This is because different languages have different requirements; for example, while English only has two cardinal plural cases (*one* and *other*), Russian has four (*one*, *two*, *few* and *many*). Keeping the format function in the string table allows a translator to rewrite the function to suit the needs of the language they're working in.

The variables used in the format function treated the same way as [inline expressions](#localising-inline-expressions): they're replaced with placeholders.

For example, the following Yarn code:

```yarn
I have [plural {$apples} one="an apple" other="% apples"]!
```

Will appear like this in the base language string table:

```
I have [plural {0} one="an apple" other="% apples"]!
```

When you generate a new string table for another language, you can rewrite this format function to suit the needs of the language you're working in.

For example, the above line can be translated into Polish, and use the correct grammar for pluralising the word for "apple":

```
Mam [plural {0} one="jabłko" few="% jabłka" many="% jabłek"]!
```

The format function can also be entirely removed. This is useful for situations where it's not needed; for example, in Indonesian, plural markers are not used, and so the string table for Indonesian can render the line as:

```
Saya punya {0} apel!
```
