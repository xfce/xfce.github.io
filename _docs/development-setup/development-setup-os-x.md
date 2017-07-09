---
title: "Development Setup: MacOS"
excerpt: "So you want to be a dRonin?"
---
{% include toc %}

Building on OS X is relatively easy.  Consider forking the project on GitHub before proceeding with this procedure if you intend to contribute back to the project.  (More details on this are at [Tracking Development with Git](doc:tracking-development-with-git))

## Setting up prerequesites for the build environment

{% include callout type="warning_full" title="Supported macOS Versions" text="GCS requires macOS 10.10 or newer. See the [Qt supported platforms list](http://doc.qt.io/archives/qt-5.8/supported-platforms.html#supported-configurations) for further details." %}

### Get Homebrew

Homebrew is a package manager; it's the best way to get some of the prerequisites to build dRonin.

In a [terminal](http://www.google.com/search?q=osx+terminal+tutorial) window, paste this magical incantation:

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

and then:

```
brew doctor
```

Then, you can use homebrew to install wget:

```
brew install wget
```

### Download Required Programs

Xcode. If you do not already have Xcode, the latest version can be obtained from the Apple app store.

After this, start Xcode and go through the initial setup. Once Xcode is running, go to Xcode > Preferences > Downloads > Components and install "Command Line Tools".

## Checking out the dRonin repository and building

### Clone the source code repository

First, clone the dRonin repository.  If you have your own fork, specify it on the git command line.

```
git clone git://github.com/d-ronin/dRonin.git
cd dRonin
```

### Automatic download and install of required programs

The dRonin build environment is capable of installing the rest of the tools that it needs.

### Qt build tools

{% include callout type="warning_full" title="Removing existing Qt build locations!" text="If you have brew installed qt previously, unlink it now. If you get link errors building uavobjects, this is probably what is wrong: `brew unlink qt`" %}

Next, run `make qt_sdk_install`, copy the path from the output in your terminal and paste it into the installer when prompted.

{% capture sdkinst %}
When running the qt sdk install command, you'll be told where to install qt, then the GUI installer will open. Here is what it will look like:

```
*** NOTE NOTE NOTE ***
*
*  In the GUI, please use exactly this path as the installation path:
*        /some/path/src/dRonin/tools/Qt5.8.0
*
*** NOTE NOTE NOTE ***
```

Be sure to copy the specified path into the installer when prompted for the install location!
{% endcapture %}

{% include callout type="warning_full" title="Don't install Qt to the default location!" text=sdkinst %}

### Arm cross compilation toolchain

This is easy.  Just type:

```
make arm_sdk_install
```

## Building the software

You should be ready to go. Type `make all` to compile the entire project. Type `make` to see a list of possible make arguments. Use 'make package' to create a .dmg containing everything, ready to install.

## Running GCS

Launch the gcs with `open build/ground/gcs/bin/dRonin-GCS.app` and connect to / flash your board.

## Eclipse Setup (Optional)

Extract the eclipse project:

```
pushd flight/Project/Eclipse
unzip eclipseLinuxWsp.zip -d eclipseLinuxWsp
mv eclipseLinuxWsp/.metadata ../../../
mv eclipseLinuxWsp/.cproject ../../
mv eclipseLinuxWsp/.project ../../
mkdir ../../../tools/eclipseWorkspace
```

Install eclipse, use the `Eclipse Installer`: https://eclipse.org/downloads/

Choose the `Eclipse IDE for C/C++ Developers` when prompted

When Eclipse starts, choose the folder you created in `[Your Project Root]/tools/eclipseWorkspace` as the workspace directory.

Then choose `File` -> `Import` and pick `Import an Existing Project`. Choose your project root directory, which is the same place you checked out the project with git.

You'll see the two projects `android gcs` and `flight` appear. Hit `Finish` and you're good to go!