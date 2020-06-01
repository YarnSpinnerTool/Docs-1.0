---
title: "Using the Yarn Editor"
date: 
tags: []
summary: "Learn how to work with the Yarn Editor, a visual tool for writing dialogue in Yarn Spinner."
draft: false
toc: true
dont_show_in_lists: true
weight: 5
menu: 
    docs:
        parent: "editor"        
---

The [Yarn Editor](https://github.com/YarnSpinnerTool/YarnEditor) is a visual tool for writing dialogue for Yarn Spinner. It's free and open source, and works on Windows and macOS. You can also use the Yarn Editor in the browser.

## Getting the Yarn Editor

To download the Yarn Editor, go to the project's [releases page](https://github.com/YarnSpinnerTool/YarnEditor/releases/latest), and download the version of the app based on your OS.

* For Windows users, download the .exe.
* For macOS users, download the .dmg.

To use the Yarn Editor in the browser, go to [yarnspinnertool.github.io/YarnEditor](https://yarnspinnertool.github.io/YarnEditor/). The Yarn Editor supports Google Chrome, Firefox, and Safari.

Once you've launched the Yarn Editor, it will start with a new document, which contains one node.

{{< img "yarn-editor.png" "The Yarn Editor" >}}

## Opening and Saving Documents

To save your work as a `.yarn` file, move your mouse cursor over the `FILE` menu, and choose `SAVE AS YARN`. 

{{< note >}}
While the Yarn Editor allows saving in other formats, Yarn Spinner will only work with `.yarn` format. 
{{< /note >}}

To open a `.yarn` file, move your mouse cursor over the `FILE` menu, and choose `OPEN`. 

## Creating Nodes

To create a node, click the `+ NODE` button. A new node will be added to the document.

## Removing Nodes

To remove a node from the document, move your mouse over it, and the Delete button will appear. Click on it, and the node will be deleted.

## Moving Around the Node View

To change which part of your document you're looking at, you can move the view around, and zoom in and out.

* To zoom in and out, scroll your mouse's scroll wheel.
* To move the view, hold down the Option key (Alt key on Windows), and click and drag with the left mouse button. You can also click and drag with the middle mouse button.

## Arranging Nodes

To move nodes around, click and drag on a node with the left mouse button. The node will move around.

To move multiple nodes around at the same time, click and drag with the left mouse button on any empty point in the node view, and drag to create a rectangle. When you release the mouse button, any node in the rectangle will be selected; when you click and drag any of these nodes, all of them will move together. Click outside any node to de-select all nodes.

## Editing Nodes

To edit the contents of a node, double-click on it. The editing view will appear.

{{< img  "yarn-editor-editing-node.png" "Editing a node in the Yarn Editor" >}}

In the editing view, you can modify the *title*, *tags* and *body* of a node. When you're done, click outside the editing view, and it will close.