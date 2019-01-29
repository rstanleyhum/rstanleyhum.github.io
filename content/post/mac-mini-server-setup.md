---
title: "Mac Mini Server Setup"
date: 2018-09-15T20:32:51-04:00
tags: []
draft: false
---

In this post, I will be describing setting up my mac mini as a iOS build server.

In fact, I will be eventually using Travis with GitHub and Fastlane for my Flutter project but this is the local build setup.

First, we will start with a clean install of High Sierra. Download the High Sierra Install from the AppStore and create the media with the following command:

```shell
    sudo /Applications/Install\ macOS\ High\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume --applicationpath /Applications/Install\ macOS\ High\ Sierra.app
```

Reference: https://support.apple.com/en-us/HT201372


## Using the Startup Manager

1. Press and hold the Option key immediately after turning on or restarting the Mac.

2. Release the Option key when you see the Startup Manager window

Reference: https://support.apple.com/en-us/HT202796

## Once High Sierra is re-installed

1. Check for software updates

```shell
    softwareupdate -l
```

2. Update the software

```shell
    softwareupdate -i -a
```

3. Check Remote Login status

```shell
    sudo systemsetup -getremotelogin
```

4. Turn on remote access.

```shell
    sudo systemsetup -setremotelogin on
```

5. Set computer name

```shell
    sudo scutil --set HostName <ComputerName>  
    sudo scutil --set LocalHostName <ComputerName>  
    sudo scutil --set ComputerName <ComputerName>  
    dscacheutil -flushcache
``` 

6. Restart

```shell
    sudo reboot
```

## Now for setting up Flutter with Fastlane

I have decided to use Flutter is the crossplatform framework. Fastlane seems to be the right tool to help with the deployment. In supports both Android and iOS with both testing and production. It also integrates with Flutter and Travis CI which means that I can eventually do the builds automatically from the cloud.

First, however, it is easier to setup on a local machine.

## Setup Flutter on MacOS

Reference: https://flutter.io/setup-macos/

1. Get the Flutter SDK

```shell
    curl -O https://storage.googleapis.com/flutter_infra/releases/beta/macos/flutter_macos_v0.7.3-beta.zip
```

Note: curl -O is the macOS equivalent of wget

2. Extract to a development tools path

```shell
    mkdir ~/bin
    cd ~/bin
    unzip ~/Downloads/flutter_macos_v0.7.3-beta.zip
    echo 'PATH=/Users/<username>/bin/flutter/bin:$PATH' >> ~/.bashrc
    source ~/.bashrc
```

NB: This doesn't work because ssh doesn't automatically load .bashrc. I suspect that you need .profile.


3. Run flutter doctor

```shell
    flutter doctor -v
```


## Install Xcode

I could get the Command Line Tools to install without using the AppStore but I couldn't figure out how to do it for the full Xcode which Flutter needs.

So you have to do it from the GUI. Eventually it will all be in the cloud and subsequent projects will not need the local installation.

Once you download the Xcode from the AppStore, you may need to agree to the Xcode/iOS license.

```shell
    sudo xcodebuild -licence
```

I had to run the XCode from the GUI to get the additional components to load.

Now I can start up the iOS simulator.

You should have __/Applications/Xcode.app/Contents/Developer__ as the `xcode-select -p` value.

You need to setup the iOS simulator


Controlling the iOS Simulator from the Command Line

Reference: https://medium.com/xcblog/simctl-control-ios-simulators-from-command-line-78b9006a20dc



## Install homebrew to facilitate deployment of Flutter app to a physical iOS device locally.

Go to https://brew.sh/

```shell
    brew update
    brew install --HEAD libimobiledevice
    brew install ideviceinstaller ios-deploy cocoapods
    pod setup
```
