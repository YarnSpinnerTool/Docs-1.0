---
title: "Markup"
date: 
tags: []
summary: "Learn how to modify your text with attributes."
draft: false
toc: true
weight: 5
menu: 
    docs:
        parent: "writing"
---

Markup allows you to add attributes into your text, like `[a]hello[/a]`. These attributes can be used by your game to do things like change the formatting of the text, add animations, and more.

When text is parsed, the tags are removed from the text, and you receive information about
the range of the plain text that the attributes apply to.

## Attributes

Attributes apply to ranges of text:

```yarn
Oh, [wave]hello[/wave] there!
```

The `Dialogue.ParseMarkup` method will take this text, and produce two
things: the _plain text_, and a collection of _attributes_. The plain
text is the text without any markers; in this example it will be:

```yarn
Oh, hello there!
```

Attributes represent ranges of the plain text that have additional
information. They contain a _position_, a _length_, and their _name_, as
well as their _properties_.

In this example, a single attribute will be generated, with a position
of 4, a length of 5, and a name of "wave".

Attributes are opened like `[this]`, and closed like `[/this]`.

### Overlapping Attributes

Attributes can overlap:

You can put multiple attributes inside each other. For example:

```yarn
Oh, [wave]hello [bounce]there![/bounce][/wave]
```

You can close an attribute in any order you like. For example, this has
the same meaning as the previous example:

```yarn
Oh, [wave]hello [bounce]there![/wave][/bounce]
```

### Self-closing Attributes

Attributes can self-close:

```yarn
[wave/]
```

A self-closing attribute has a length of zero.

### The Close-All Marker

The marker `[/]` is the _close-all_ marker. It closes all currently open
attributes. For example:

```yarn
[wave][bounce]Hello![/]
```

### Properties

Attributes can have properties:

```yarn
[wave size=2]Wavy![/wave]
```

This attribute 'wave' has a property called 'size', which has an integer
value of 5.

### Short-hand Properties

Attributes can have short-hand properies, like so:

```yarn
[wave=2]Wavy![/wave]
```

This attribute 'wave' has a property called 'wave', which has an integer
value of 2. The name of the attribute is taken from the first property.

### Property Types

Properties can be any of the following types:

- Integers
- Floats
- 'true' or 'false'
- Strings

Single words without quote marks are parsed as strings. For example, the two following lines are identical:

```yarn
[mood=angry]Grr![/mood]
[mood="angry"]Grr![/mood]
```

### Whitespace Trimming

If a self-closing attribute has white-space before it, or it's at the
start of the line, then it will trim a single whitespace after it. This
means that the following text produces a plain text of "A B":

```yarn
A [wave/] B
```

If you don't want to trim whitespace, add a property `trimwhitespace`,
set to `false`:

```yarn
A [wave trimwhitespace=false/] B (produces "A  B")
```

## The `nomarkup` Attribute

The `nomarkup` attribute makes the parser ignore any markup characters
inside it.

If you want to include characters like `[` and `]`, wrap them in the
`nomarkup` attribute:

```yarn
A [nomarkup];][/nomarkup] B (produces "A ;] B")
```

## The `character` Attribute

The `character` attribute is used to mark the part of the line that identifies the character that's speaking. 

Yarn Spinner will attempt to add this character for you, by looking for character names in lines that look like this:

```yarn
CharacterA: Hello!
CharacterB: Oh, hi!
```

The markup parser will mark everything from the start of the line up to
the first `:` (and any trailing whitespace after it) with the
`character` attribute. This attribute has a property, `name`, which
contains the text from the start of the line up to the `:`. If a `:`
isn't present, or a `character` attribute has been added in markup, it
won't be added.

This means that the example above is treated the same as this:

```yarn
[character name="CharacterA"]CharacterA: [/character]Hello!
[character name="CharacterB"]CharacterB: [/character]Oh hi!
```

You can use this to trim out the character names from lines in your game.

## Markup and Format Functions

{{<note>}}
This section only applies to users who are migrating from versions of Yarn Spinner prior to 2.0.
{{</note>}}

Markup replaces Yarn Spinner v1.1's format functions. Format
functions are now implemented as self-closing markers; the LineParser
class's `RegisterMarkerProcessor` method allows you to register certain
markers as "replacing" markers, which the line parser will call out to
your code to fetch the text to insert into the final, composed line. The
`nomarkup`, `select`, `plural`, and `ordinal` markers are implemented in
this way.

This means that your format functions need to be changed slightly to
match the new format. 

Previously:

```yarn
[select {$gender} m="he" f="she" nb="they"]
[plural {$pies} one="pie" other="pies"]
[plural {$position} one="%st" two="%nd" few="%rd" other="%th"]
```

Now:

```yarn
[select value={$gender} m="he" f="she" nb="they" /]
[plural value={$pies} one="pie" other="pies" /]
[plural value={$position} one="%st" two="%nd" few="%rd" other="%th" /]
```

Additionally, you will need to set the `Dialogue.LocaleCode` property to
a locale code, like `en`.
