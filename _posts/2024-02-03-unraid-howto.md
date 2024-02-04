---
title: unRAID HOWTO
date: 2024-02-03 20:55:00 +0500
categories:
  - howto
  - unraid
tags:
  - setup
  - install
---
# unRAID First Time Setup

Much of this process is documented elsewhere. This is my spin on the process. I'll try and link the sources to the supporting Docs or Articles that I've found useful. Much of the stuff that I suggest is my preference after using unRAID for a while. My server is a multi-purpose machine hosting Gaming VMs, Docker Containers, NAS and serving media via Emby. 

**Disclaimer:** Proceed with caution. I enjoy tinkering and learning. Everything I know is subject to revision. This is a work in progress. 

##### Reset OS / Wipe All Data - [HOWTO](https://forums.unraid.net/topic/119221-reset-unraid-to-factorydefault-settings/?do=findComment&comment=1090314https:/)


## Install Unraid
[unRAID Install Guide](https://docs.unraid.net/unraid-os/getting-started/quick-install-guide/)


## Setup Array and Cache Pools
Setup [unRAID Array](https://docs.unraid.net/unraid-os/getting-started/quick-install-guide/#assigning-devices-to-the-array-and-pools)
- [Parity](https://docs.unraid.net/legacy/FAQ/Parity/) (Optional but suggested. Can be added later...)
	- Parity is used by Unraid to protect against data loss. If a drive in the array fails, the data on the other drives can be combined with the parity data to reconstruct the missing data
- Adjust the number of visible `Slots` under `Array Devices` to the number of disks you'll be adding.
- Assign Array Disk(s)

![](/assets/posts/2024-02/20231103_234828_image.png)


## Create a [Cache Pool](https://docs.unraid.net/unraid-os/manual/storage-management/#why-use-a-pool)
- Click `ADD POOL` > name the pool and click `ADD`
- If no other disks are being added to Cache or Array, proceed to start the Array

![](/assets/posts/2024-02/20231103_234939_image.png)
![](/assets/posts/2024-02/20231103_235126_image.png)


## Start the Array
![](/assets/posts/2024-02/main-array.png)

**The default `Filesystem` is `XFS` for the Array and `BTRFS` for the Cache Pool. ** If another file system is preferred, [see this article](https://docs.unraid.net/unraid-os/manual/storage-management/#selecting-a-file-system-type).

## Format Array Disks
- After starting the Array, the installed disks must be formatted. 
- Tick the box `Yes, I want to do this` > Agree to the warning
- Click Format
- Depending on the size of the disks, this may take a several minutes.
- `Started` should appear below `Array Operation`

![](/assets/posts/2024-02/20231103_235532_image.png)
![](/assets/posts/2024-02/20231103_235703_image.png)
![Array Started](/assets/posts/2024-02/20231103_235912_image.png)



# Plugin Suggestions
Install Community Applications
- APPS > Install Community Applications
- 👀️ Wait for icon to display *DONE* when installing APPS! 👀️
	![community apps not installed](/assets/posts/2024-02/20231103_230935_image.png)
- Click `DONE` > Agree to Disclaimer..

- User Scripts
- Fix Common Problems
- Appdata Backup
- Appdata Cleanup
- Dynamix File Integrity
- Dynamix File Manager
- Dynamix System Information
- Dynamix System Statistics
- GUI Search
- Tips and Tweaks
- Unassigned Devices
- Unassigned Devices Plus
- Unassigned Devices Preclear
- unRAID Connect
- User Notes


## Dark Mode
- Install `Theme Engine`
- Navigate to `PLUGINS` > Search > `theme engine` > `Install`
	![install theme engine](/assets/posts/2024-02/20231103_231302_image.png)
	
Activate `Dark Mode`
- Toolbar > Plugins > `Theme Engine`
-  BASE THEME > `Black`
- Apply

## Docker
Unraid > Settings > Docker > DO NOT START YET
- Change `Docker vDisk size:` This can be increased in the future, however it cannot be reduced.
- Enable Docker `Yes` > `Apply`
- `Docker` should now be displayed within the toolbar.

![](/assets/posts/2024-02/20231104_014919_20231104_0006docker_image.jpg)


## VM Manager 
Unraid > Settings > [VM Manager](https://docs.unraid.net/unraid-os/manual/vm-management/)
- Click `Advanced View`
- - Enable VMs `YES`
- Libvirt vdisk size: `50` GB
- Download VirtIO drivers
- Additional Settings can be changed in the future. Testing a few options such as `PCIe ACS override` must be modified if passing through a PCIe device.
- Status: `Running` should appear in the top left.
- `VMS` should now be displayed within the toolbar.
![](/assets/posts/2024-02/20231104_015446_image.png)
- ISOs for VMS should be downloaded to /mnt/user/isos. Additionally, the VIRTIO drivers disk is also needed when installing a Windows VM.
- Now that Docker, VMs and Dark mode has been enabled, we can proceed to ensuring our VMs and Docker AppData are stored on Cache only.

  

### Basic Tuning - Cache Directories 
- Unraid > Shares > `appdata` > Primary Storage (for new files and folders): `Cache`
- [Secondary Storage:](https://docs.unraid.net/unraid-os/manual/storage-management/#change-pool-raid-levels) `none`
	- I prefer to keep VMs, ISOs and Docker appdata in a Cache Pool. This method does not provide Parity Protection, however, running the Cache Pool is RAID0, will provide redundancy. 
- `Apply`
- Repeat for VMs, ISOs and Docker appdata
![](/assets/posts/2024-02/20231104_024359_image.png)

### Optional
SMB/NFS Shares
- Export: `YES` > Apply
- Reminder: `Public` assumes you trust everyone who connects to your LAN. Keep this in mind when sharing other directories that may contain certain `linux ISOs`.
- ![](/assets/posts/2024-02/20231104_024300_image.png)
After applying similar settings to `ISOs`, `Domains` and `appdata`, data should be written to and stored on Cache only
![](/assets/posts/2024-02/20231104_030747_image.png)

#### Creating additional shares
Suggested directories would include:
- `media`
	- movies, shows, music
- `downloads`
	- subfolders to include Web Downloads, Torrents and Usenet

For these directories, initial data is stored on the Cache and then written to the Array by Mover during periods of inactivity. To fine tune mover, install the plugin `Mover Tuning`. IMO this is not a requirement for new users.
- Unraid Forums - [Mover Tuning](https://forums.unraid.net/topic/70783-plugin-mover-tuning/)

  



  
  

##### Unraid Suggested Plugins
- Ibracorp Top 20 Plugins (2021) [https://youtu.be/cZTWC_z9rKs](https://youtu.be/cZTWC_z9rKshttps:/)
- SpaceInvader One's Must Have Plugins [https://youtu.be/su2miwZNuaU](https://youtu.be/su2miwZNuaU)

  

##### Unraid Content Creators
- SpaceInvader One [https://www.youtube.com/@SpaceinvaderOne](https://www.youtube.com/@SpaceinvaderOne)
	- The Godfather. The greatest.
- IBRACORP [https://www.youtube.com/@IBRACORP](https://www.youtube.com/@IBRACORP)
- SPX Labs [https://www.youtube.com/@SPXLabs](https://www.youtube.com/@SPXLabs)
- Byte My Bits https://www.youtube.com/@Bytemybits
- Techhut https://www.youtube.com/@TechHut


  
#### Help
- To provide additional information realted to items within the Unraid UI, click the `question mark` in the upper right hand portion of the `toolbar`. Doing so activates `Help`. Repeating the action removes the additional context menus.

- ![](/assets/posts/2024-02/20231104_020930_image.png)
	![](/assets/posts/2024-02/20231103_231022_image.png)

#### Set Static IP
Settings > Network Settings > `IPv4 address assignment`
- Additional settings like `DNS` & `VLANs` can be modified
![](/assets/posts/2024-02/20231104_000600_image.png)

  