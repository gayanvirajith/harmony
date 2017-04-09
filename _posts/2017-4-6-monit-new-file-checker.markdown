---
layout: post
title:  "Verifying New File Creation With Monit"
date:   2017-4-6 21:00:00
permalink: 'blog/verifying-new-file-creation-with-monit'
---

I have recently been experimenting with [monit](https://mmonit.com/monit/), a lightweight and highly configurable tool for monitoring your system(s).  
Monit makes it easy to ensure that processes are always running, that network services are reachable, and that directories are mounted at all times.  

To facilitate a rudimentary nightly, off-site backup, I am currently using 2 crons:

`Cron 1` runs at 1am and uses `SCP` over `SSH` to retrieve a `tar.gz` archive from a remote server. This tar is saved to a local `backups` directory.  
`Cron 2` runs at 2am, purging any archives that are older than 7 days from the local `backups` directory, as to conserve storage space.

I wanted to create a monit rule to ensure that, every day, a new archive has been downloaded and saved to the local `backups` folder.
Initially, I used the following monit rule, which checks the `updated_at` timestamp of the `backups` directory, ensuring that it has changed within the past 24 hours

```
check directory backup with path /media/PHILIPS/backups
  if timestamp > 24 hours then alert
```

Unfortunately, I soon discovered that the `updated_at` timestamp was changing each day, even when the download from the remote server had failed. As you've probably guessed, the process of purging old backups each night (`Cron 2`) was causing the `updated_at` timestamp to increment.

After realising that simply checking the `timestamp` attribute of the directory was insufficient, I searched for a way of verifying directory *contents*, instead.  
I found that monit can shell out to an arbitrary program, and can determine success/failure based on the program's exit value or textual output.  
I wrote a small Ruby script to scan a directory for recently created file(s), returning an exit code of 0 (success) if a new file is found, or a code of 1 (error) if no new files were detected.

Here's the ruby script and modified monit rule that makes it all possible.

```
check program mindful_backup with path "/home/pi/new_file_checker.rb"
    if status != 0 then alert
```
<script src="https://gist-it.appspot.com/github/dvjones89/ruby-tools/blob/master/new_file_checker.rb"></script>

![Monit OK Message](/assets/images/2017/monit_ok.png){:.aligncenter}
![Monit Failure Message](/assets/images/2017/monit_fail.png){:.aligncenter}

Hopefully this helps someone out there.
