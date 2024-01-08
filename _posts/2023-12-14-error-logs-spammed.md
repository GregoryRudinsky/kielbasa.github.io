---
title: Unraid Error Logs Spammed
date: 2023-12-14 17:55:00 +0500 # date-time-offset est=+5
categories: [errors, unraid]
tags: [troubleshooting, btrfs]
--- 

## Unraid logs spammed

![errors](/assets/posts/2024-01/20231214_133759_log-error-sample.png)
Symptoms: sluggish GUI, log full, constant disk read/write errors.

Cause: **Me**

- Unclean shutdowns, a failed disk, bad sata cable? And possibly other bad practices.

### Solution

- Learn a bit more about how unRAID operates.
- Stop Docker
- Stop Array
- Watch 'htop' to see whats hanging
- Scrub cache pool
  - Increase frequency of pool scrub
    - Main > Cache Pool > Scrub Status
- Verify Trim Settings
  - `Trim` and `Partity Check` settings can be found in Scheduler
    - Settings > User Preferences > Scheduler

#### Several docs that contributed to the solution  

<https://forums.unraid.net/topic/135435-should-i-be-scrubbing-and-balancing-btrfs-nvme-cache-pool/?do=findComment&comment=1232225>

##### Unraid man BTRFS Scrub

<https://docs.unraid.net/legacy/FAQ/check-disk-filesystems/#btrfs-scrub>

##### Dealing with Unclean Shutdowns

<https://forums.unraid.net/topic/69868-dealing-with-unclean-shutdowns/>

##### Scrub Cache Pool

<https://forums.unraid.net/topic/135435-should-i-be-scrubbing-and-balancing-btrfs-nvme-cache-pool/>
