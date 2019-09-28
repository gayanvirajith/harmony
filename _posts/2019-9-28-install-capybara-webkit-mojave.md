---
layout: post
title:  "Installing Capybara-WebKit On MacOS Mojave"
date:   2019-9-28 22:00
permalink: 'blog/install-capybara-webkit-mojave'
---

I've recently upgraded to a Macbook Pro 2019 and need to setup my local development environment in MacOS Mojave. As a Ruby on Rails developer, there's a shortlist of Ruby gems that always seem to put a fight when I run `bundle install` and the `capybara-webkit` gem has proven to be one of the worst. This post outlines the steps needed to install the `capybara-webkit` gem on a fresh copy of MacOS Mojave (10.14.6), hopefully saving you an hour of Google searching.

## Some Background

Many Ruby on Rails projects use `capybara-webkit` to power their feature specs (tests that exercise their web app, including javascript, within a real browser environment). The `capybara-webkit` gem depends on some software called [Qt](https://en.wikipedia.org/wiki/Qt_(software)) and won't compile on MacOS without some QT libraries (`libqt`) being present on your computer.



## Step 1: Install Qt 5.5
Our first step is to install the Qt libraries needed to compile and install `capybara-webkit`. Ordinarily we'd reach for MacOS' unofficial package manager, [Homebrew](https://brew.sh), to install our missing software, though the (newer) versions of Qt available via Homebrew no longer have the libraries that `capybara-webkit` requires. To ensure we have all the libraries we need, we must manually install an older version of Qt (version 5.5) via [this link](https://download.qt.io/archive/qt/5.5/5.5.0/qt-opensource-mac-x64-clang-5.5.0.dmg).

Once we've installed Qt 5.5, we need to add its binaries to our system's path, ensuring they can be found when we next try to build `capybara-webkit`. Depending on which Shell you're using (the Mojave default is Bash), you'll need to add the following line to your `.bash_profile` or `.zshrc` (if you're using the ZSH shell)

```shell
export PATH=$HOME/Qt5.5.0/5.5/clang_64/bin:$PATH
```
Once you've added the above line to your shell's configuration file, restart your terminal and run `which qmake` to ensure the Qt libraries are available. You should see something similar to the output below:
```shell
which qmake
/Users/your-name/Qt5.5.0/5.5/clang_64/bin/qmake
```

## Step 2: Install XCode 9.4
In order to install `capybara-webkit`, we need to compile its source code on your computer. The previous steps gave us the Qt 5.5 libraries on which we depend, though we also need some extra C++ libraries (e.g. `cstddef.h` and `limit.h` ) that don't ship with a fresh copy of MacOS Mojave.

Most of the time we get our compilation headers from XCode.app, free software developed by Apple and available via the Apple App Store. Unfortunately, the specific C++ libraries needed to build `capybara-webkit` no longer ship with recent versions of XCode (version 10 and onwards), meaning we need to downgrade to XCode 9.4.  

Downgrading is pretty straight forward, simply grab XCode 9.4 from [Apple's Developer pages](https://developer.apple.com/download/more/?&name=Xcode), extract the `.zip` archive and copy the `Xcode` app into your `/Applications` directory. Finally, restart your terminal and execute the following commands to ensure XCode 9.4 is registered in your terminal and that the terms and conditions have been accepted.
```shell
sudo xcode-select -switch /Applications/Xcode.app/Contents/Developer
sudo xcodebuild -license agree
```

## Step 3: Install capybara-webkit via Bundler
You'll be glad to know that the hard work is now done. We've installed the development libraries for Qt along with the C++ headers needed for compilation, meaning you should be able to build and install capybara-webkit:

```
gem install capybara-webkit
```
I have this helps and happy coding!
