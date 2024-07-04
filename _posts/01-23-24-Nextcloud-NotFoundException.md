---
title: Nextcloud-NotFoundException
date: 2024-01-23 20:55:00 +0500
categories:
  - nextcloud
  - logs
  - errors
tags:
  - dumb
  - unraid
  - occ
---
## Nextcloud Errors 

Errors were building in my `Logs`. I migrated my `Users` to a Nextcloud AIO instance and subsequently deleted the export. Logs were being spammed due to a missing file. This happens when a file is removed outside of the Nextcloud UI and the DB is not informed of the change. 

To view the `Errors`:
1. Profile > Administrative Settings > `Security & Setup Warnings`
2. `Administrative Settings`,  navigate to `Logging`
![](/assets/posts/2024-01/01-23-24/1-23-24_nc-error-missing-index.png)

![](/assets/posts/2024-01/01-23-24/1-23-24_nc-error-notfoundexception-pre.png)

### Solution
1. Reindex files for `User`
```bash
docker exec -it nextcloud occ files:scan --all
```

2. Add missing indices
```bash
root@Ashla-234:/# docker exec -it nextcloud occ db:add-missing-indices
Adding additional mail_messages_msgid_idx index to the oc_mail_messages table, this can take some time...
oc_mail_messages table updated successfully.
```

3. Backup Logs 
	- Optional
	
4. Delete Logs
```bash
root@Ashla-234:/mnt/user/nextcloud-data# rm -f nextcloud.log
```

5. Restart Nextcloud
```bash
root@Ashla-234:/# docker restart nextcloud
```

6. Confirm errors were resolved
![](/assets/posts/2024-01/01-23-24/1-23-24_nc-error-notfoundexception-post2.png)

7. An additional optional step is to disable the `User Migration` plugin
![](/assets/posts/2024-01/01-23-24/1-23-24_nc-error-user-migration.png)


#### Recap of commands in `Terminal`
![](/assets/posts/2024-01/01-23-24/1-23-24_nc-error-terminal-commands.png)


