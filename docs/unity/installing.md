---
title: "Installing Yarn Spinner for Unity"
date: 2019-12-11T19:11:07+11:00
tags: []
draft: false
summary: "Learn how to install Yarn Spinner in a Unity project."
menu:
    docs:
        title: "Installing"
        parent: "unity"

weight: 1
---

There are two ways to install Yarn Spinner for Unity: via the Unity Package Manager, and via a `.unitypackage` file. 

{{<note>}}
We recommend installing it via the Package Manager, because it's easier to update to new versions of Yarn Spinner as they become available, it doesn't get embedded in the source code of your game, and you have more control over what gets added to your project. 
{{</note>}}

## Install via the Unity Package Manager (recommended)

You can install the Yarn Spinner package into your project via OpenUPM, or you can do it manually by editing your project's `manifest.json` file.

### Install via OpenUPM 

Yarn Spinner is available on the [OpenUPM registry](https://openupm.com). This is the simplest way to install Yarn Spinner, and makes it easy to keep it up to date.

{{<note>}}To use OpenUPM, install [openupm-cli](https://github.com/openupm/openupm-cli) on your machine. {{</note>}}

Once OpenUPM is installed, open a terminal inside your Unity project's directory, and run the following command:

```
openupm add dev.yarnspinner.unity
```

Yarn Spinner will download and install into your project.

### Install manually

To install the package manually, follow these steps.

#### Unity 2019.3 and later

If you're using Unity 2019.3 or later, you can add the package directly:

1. In Unity, open the Window menu, and choose Package Manager.
2. Click the `+` button, and choose "Add package from git URL".

{{< img "v1.1/installing-package-manager-git-url.png" "Adding a package using a Git URL." >}}

3. Enter the following URL: **`https://github.com/YarnSpinnerTool/YarnSpinner.git#upm`**.
4. The project will download and install.

#### Unity 2018.4 to Unity 2019.2

If you're using an earlier version of Unity, you'll need to add the package by updating your project's manifest file:

* Go to the folder that your project is in.
* Open the `Packages` folder.
* Open the file `manifest.json` in your text editor (for example, Visual Studio Code.)
* Update the file to include the following line (don't forget the comma at the end!):

<pre>
<code class="json">{
  "dependencies": {
    <b>"dev.secretlab.yarnspinner": "https://github.com/YarnSpinnerTool/YarnSpinner.git#upm",</b>
    "com.unity.2d.sprite": "1.0.0",
    ...
</code>
</pre>

* Return to Unity, and Yarn Spinner will download and install.

## Install as a file

If you don't want to install Yarn Spinner as a package, you can download it as a `.unitypackage` file, and extract it into your project.

To download and install the file, follow these steps:

1. In Unity, open the Window menu, and choose Package Manager.
2. Select **Text Mesh Pro**, and click the Install button. (This package is required for Yarn Spinner to work.)
3. Next, open your browser, and go to the [most recent release]({{< param githubUrl >}}/releases/latest) of Yarn Spinner on GitHub.
4. Download the `.unitypackage` for that release.

{{< img "v1.0.1/download.png" "The release page on GitHub." >}}

5. Open the `.unitypackage`. Unity will ask which files you want to add. In almost every case, you'll want to import all files.

{{< img "v1.0.1/importing.png" "Importing the package." >}}
6. Click Import.

## Next Steps

Once you've installed Yarn Spinner, you're ready to start using it! You may want to start with the [tutorial]({{< ref "tutorial.md" >}}).