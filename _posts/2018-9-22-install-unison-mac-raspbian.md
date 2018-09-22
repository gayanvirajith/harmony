---
layout: post
title:  "Installing Unison On Raspbian (Jessie) And Mac OS 10.11.6 (El Capitan)"
date:   2018-9-22 14:00
permalink: 'blog/install-unison-mac-raspbian'
---

I recently had a requirement for directional file synchronisation between 2 hosts; a Raspberry Pi 3 running Raspbian (Jessie), and an Apple Mac running Mac OS 10.11.6 (El Capitan). I initially hoped to use rsync running on a cronjob, though after Googling around, I quickly learned that rsync prefers to sync in one direction only, and that many people reach for a tool called [Unison](https://www.cis.upenn.edu/~bcpierce/unison/) to allow for bidrection sync over SSH. Sounds good, right?  

Initially I installed the latest Unison binaries available through the system package managers, Homebrew on OS X and APT on Raspbian. Unfortunately, the installed versions were wildly different across the different hosts, which raised an error similar to the following when trying to kick off a sync:

```shell
Received unexpected header from the server. Expected Unison X.XX but recived Unison Y.YY
```

After realising that neither Homebrew nor APT had a common back-dated version of Unison in their repositories, I resorted to building the latest version of Unison from source. Unison is written in a language called [OCaml](https://ocaml.org/) and so, before you can successfully build Unison, OCaml needs to be installed on the host. Again, I used Homebrew and APT to install the latest available versions of OCaml, though immediately encountered errors when trying to build Unison, such as the below:
```
unison ocamlopt unknown option unsafe string
This expression has type bytes but an expression was expected of type string with OCaml version 4.07.0. Happily, there 
```

Eventually I discovered that, in order to build Unison correctly, you're best sticking with OCaml version 4.07.0. Happily, there's an OCaml package manager called opam, which makes it easy to install and switch between different versions of the OCaml language. Here are the command I ran to install OCaml 4.07.0 and then build Unison 2.5.1 on OS X (El Capitan) and Raspbian (Jessie). I imagine these commands would work on other variants of OS X and Debian, too.

OS X El Capitan Commands
```shell
brew install opam m4
opam init -y --compiler=4.05.0
eval `opam config env`
wget http://www.cis.upenn.edu/\~bcpierce/unison/download/releases/unison-2.51.0/unison-2.51.0.tar.gz
tar -xvzf unison-2.51.0.tar.gz
cd src
make UISTYLE=text
sudo cp ./unison /usr/local/bin
```

Raspbian (Jessie) Commands
```shell
sudo apt-get install opam m4
opam init -y --compiler=4.05.0
eval `opam config env`
wget http://www.cis.upenn.edu/\~bcpierce/unison/download/releases/unison-2.51.0/unison-2.51.0.tar.gz
tar -xvzf unison-2.51.0.tar.gz
cd src
make UISTYLE=text
sudo cp ./unison /usr/local/bin
```

Huge thanks to [janestreet.com](https://github.com/janestreet/install-ocaml), whose [instructions on GitHub](https://github.com/janestreet/install-ocaml) explain how to get up and running with opam, which is a life-saver. Once you've got opam and OCaml installed, the final few commands to extract and build the source should give you a beautiful unison binary that can be copied to your path and tested using `unison -version`.

I hope this helps someone out there.
