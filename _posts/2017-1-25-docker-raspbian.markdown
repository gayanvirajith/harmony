---
layout: post
title:  "[SOLVED] Docker on Raspbian - No Available Network"
date:   2017-1-25 23:15:00
permalink: 'blog/docker-raspbian-no-available-network'
---

I was excited to learn that Docker now officially supports rasbian on the Raspberry Pi 3.

Keen to try it out, I found Docker's handy one-liner to install `docker-engine` on my RPi.  
Warning: There's an inherent risk whenever you pipe an arbitrary shell script into your bash interpreter.  
I recommend `curl`ing the script into a file, first, so you can sanity check the commands before they're executed on your box.

```
curl -sSL https://get.docker.com/ > script.sh
cat script.sh
```

TLDR: The shell script adds the docker repository to your list of aptitude sources, runs `apt-get update`, followed by the install command, `apt-get install docker-engine`.  
If you're happy that the script looks reasonable, make it executable and kick it off:

```
chmod +x script.sh
./script.sh
```

For me, the installation appeared to complete successfully, until it processed the triggers for `systemd`, the raspbian manager responsible for actually starting the `docker` process.
Inspection of the logs showed that docker was failing to start, complaining about no unavailable network.

```
Jan 24 23:47:13 cavepi dockerd[9310]: Error starting daemon: Error initializing network controller: list bridge addresses failed: no available network
Jan 24 23:47:13 cavepi systemd[1]: docker.service: main process exited, code=exited, status=1/FAILURE
Jan 24 23:47:13 cavepi systemd[1]: Failed to start Docker Application Container Engine.
Jan 24 23:47:13 cavepi systemd[1]: Unit docker.service entered failed state.
```

After Googling around, I found several reports of similar (though not identical) errors, that had been traced back to openvpn having been running, at the time of install.  
Indeed, I **did** have an openvpn server running at the time pf install, so I tried halting the openvpn server, uninstalling `docker-engine` (plus its dependencies) from my RPi, and kicking off the installer again:

```
sudo systemctl stop openvpn
sudo apt-get remove --purge docker-engine
sudo apt-get autoremove
./script.sh
```

Unfortunately, even with the openvpn server disabled, I still faced the same error message:

```
Error starting daemon: Error initializing network controller: list bridge addresses failed: no available network
```

Eventually, I discovered that, despite passing the `--purge` flag to `apt-get remove`, some files had stuck around in the `/var/lib/docker` directory.  
I believe these files still had a reference to my network from when the openvpn (`tun0`) interface was present, which meant `openvpn` was still biting me.  
Alas, removing the rogue directory, and then repeating the steps to purge and clean install `docker`, resulted in a successful install 🎉.

```
rm -fr /var/lib/docker
sudo apt-get remove --purge docker-engine
sudo apt-get autoremove
./script.sh
```

I hope this helps someone else, out there.
