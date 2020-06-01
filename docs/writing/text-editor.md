---
title: "Using a text editor"
date: 
tags: []
summary: "Learn how to use a text editor, like Visual Studio Code or Sublime Text, to write Yarn Spinner content."
draft: false
toc: true
dont_show_in_lists: true
weight: 7
menu: 
    docs:
        parent: "editor"
---

Yarn files are plain text files, which means you can use any text editor you like to work with them. This can be useful if you're want to use the same editor or integrated development environment that you use to work on other parts of your game.

{{< note >}}
If you use [Visual Studio Code](https://code.visualstudio.com), we provide an [extension](https://marketplace.visualstudio.com/items?itemName=SecretLab.yarn-spinner) that adds support for Yarn Spinner.
{{< /note >}}

## Creating a new `.yarn` file

To create a new `.yarn` file in your text editor, create a new empty file, and add the following text to it:

```yarn
title: Start
---
Hello, world!
===
```

This example file contains a single node, called `Start`, which itself contains a single line ("Hello, world!").

Save this file as (for example) `Demo.yarn`. The file is now ready to be used in your game.

## Yarn Spinner file structure

In `.yarn` files, nodes are divided into two sections: the *headers*, and the *body*. Headers contain information about a node, while the body contains the actual text of the node.

At least one header is required: the `title` header, which defines the name of the node.

If your node has tags, they're also stored in the headers. For example, the following node has two tags, `tag1` and `tag2`:

```yarn
title: Start
tags: tag1 tag2
```

Headers are divided from the body with a `---` (three dashes). 

The body then runs for as many lines as you like, up to a line that contains `===` (three equals signs); this indicates the end of the node. After this, another node can appear, starting with the headers.

## Working with other editors

`.yarn` files can be opened and modified from any editor. If you create a file in a text editor, you can open it in the [Yarn Editor]({{< ref "yarn-editor" >}}), and vice versa. 

Certain editors might add additional headers to nodes; for example, the Yarn Editor includes additional headers that define the position of the node.