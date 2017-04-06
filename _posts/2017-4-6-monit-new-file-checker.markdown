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

Unfortunately, I soon discovered that the `updated_at` timestamp was changing each day, even when the download from the remote server had failed. As you've probably guessed, the process of purging old backups each night (`Cron 2`) was causing the `updated_at` timestamp to increment each night.

After realising that simply checking the `timestamp` attribute of the directory was insufficient, I searched for a way of verifying directory *contents*, instead.  
I found that monit can shell out to an arbitrary program, and can determine success/failure based on the program's exit value or textual output. As such, I wrote a small Ruby class to scan a directory for recently created file(s), returning an exit code of 0 (success) if a new file had been found, or a code of 1 (error) if no new files were detected.

Here's the ruby class and modified monit rule that makes it all possible.

```
check program mindful_backup with path "/home/pi/new_file_checker.rb"
    if status != 0 then alert
```

```ruby
#!/usr/bin/env ruby

class NewFileChecker
  # The directory to be checked for the presence of new file(s)
  CHECK_DIRECTORY="/media/PHILIPS/backups"
  # The maximum accepted time since last file creation. Exceeding this limit results in a check failure.
  CHECK_LIMIT=86400 #seconds (24 hours)

  def self.check
    creation_dates = Dir.entries(CHECK_DIRECTORY).map do |entry|
      next if [".",".."].include?(entry) # we're not interested in the navigation symbolic links
      absolute_path = File.join(CHECK_DIRECTORY, entry)
      File.ctime(absolute_path)
    end.compact

    max_creation_date = creation_dates.max
    seconds_since_creation = (Time.now - max_creation_date).to_i

    if seconds_since_creation < CHECK_LIMIT
      puts "PASS: Latest file created #{max_creation_date}"
      exit(0)
    else
      puts "FAIL: Latest file created #{max_creation_date}"
      exit(1)
    end
  end
end

if __FILE__ == $0
  NewFileChecker.check
end
```

![Monit OK Message](/assets/images/2017/monit_ok.png){:.aligncenter}
![Monit Failure Message](/assets/images/2017/monit_fail.png){:.aligncenter}

Hopefully this helps someone out there.
