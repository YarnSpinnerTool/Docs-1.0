---
title: "Expressions and Variables"
date: 
tags: []
summary: "Learn how to work with variables, and how to use them in your code."
draft: false
toc: true
weight: 4
menu: 
    docs:
        parent: "writing"
---

The Yarn language is a full programming language, which means it has support for writing complex expressions that let you control how the dialogue in your game works. In this document, you'll learn how to write expressions, and where you can use them.

## Expressions and `if` statements

An `if` statement is used to control whether a collection of content is shown or not. When you write an `if` statement, you provide an *expression*, which is evaluated; if that expression evaluates to `true`, then the contents of the `if` statement are run.

For example, consider this code:

```yarn
<<if $gregg_day_intro is 0>>
    Gregg: Sup duder. 
    Mae: hey.
<<endif>>
```

The expression being used here is `$gregg_day_intro is 0`. In this expression, the value of the variable `$gregg_day_intro` is being compared to the number `0`, using the `is` [operator](#operators). If they're the same, the expression evaluates to `true`, and the lines are run.

## Variables

You can store information about the state of your game in *variables*. Variables store information; more specifically, they can store text, numbers, true or false values, or `null` (which represents 'no value').

To set the value of a variable, you use the `set` keyword. For example:

```yarn
Mae: Oh god. Steve Scriggins.
<<set $suspect_steve to 1>>
Lori: Yeah. Him.
```

This sets the variable `$suspect_steve` to the number 1.

If a variable hasn't been set, it will have the value `null`.

### Variables and your game

Yarn Spinner doesn't manage the storage of information in variables itself. Instead, your game provides a *variable storage* object to Yarn Spinner before you start running dialogue. 

When Yarn Spinner needs to know the value of a variable, it will ask the variable storage object you've given it. When Yarn Spinner wants to set the value of a variable, it will provide the value and the name of the variable. In this way, your game has control over how data is stored.

{{<note>}}
For more information on variable storage, and how your game integrates with Yarn Spinner, see {{<xref "/docs/unity/components">}}.
{{</note>}}

## Values

In addition to variables, expressions work with *values*. These are things like numbers, strings, and `true` and `false`. 

Any value can be stored in a variable, in addition to being used in an expression.

Yarn Spinner also has a special value, called *null*. This value represents "no value"; any variable that hasn't yet been assigned a value will be `null`.

## Operators

To work with variables and values, you combine them with *operators*. Operators are things like the arthimetic operations `+` (add) and `-` (subtract), comparison operators like `<` (less than) and `==` (equal to), as well as logical operations like `and` and `or`.

To see the complete list of operators in Yarn Spinner, see {{<xref "/docs/syntax" "expression">}}.

## Inline expressions

An expression can be embedded inside a [line]({{<ref "nodes-and-content.md#lines">}}). When you do this, the result of that expression is embedded in the text of the line, which is shown to the user.

For example, if you store the number of times the player has spoken to a character in the variable `$times_spoken`, you can display it like this:

```yarn
Player: Hi!
Character: You've spoken to me {$times_spoken} times today!
```

{{<note>}}
To make your lines have correct grammar, you can use [format functions]({{<ref "syntax.md#format-functions">}}) to select content in a line based on an expression.
{{</note>}}