+++
title = "Creating ssh keys for the master key"
date = 2018-04-26T20:37:43-04:00
draft = false
+++


# Things that you need

1. 1 Raspberry Pi Zero 1.3 (don't get the WiFi edition because we want to use it as a airgapped computer)
2. 3 USB sticks (I have 8GB but you don't need more than 1 GB because it will be used for backup of the ssh-keys)
3. 2 Micro SD at 8GB with adapter.
4. 1 Mini HDMI to HDMI cable
5. 1 USB hub
6. 1 USB keyboard
7. 1 Monitor with HDMI input
8. 1 USB 8GB stick


Process:

1. Download Windows 10 on USB on main computer.
1a. Download Rufus
1b. Confirm that Rufus is okay
2. Place on USB Key for installation
2a. Place rufus on USB key
3. Install Windows 10 on separate laptop with clean installation.
3a. Place rufus on separate laptop.
4. Download Raspbian Stretch (2018-04-26). At the time of writing this guide the tools that are necessary are on Raspbian. They include:
    i. gnupg
    ii. ssh-keygen
    iii. /proc/random
5. Using rufus copy Raspbian Stretch onto MicroSD #1
6. Using rufus copy Raspbian Stretch onto MicroSD #2 (backup system)
7. Connect keyboard, monitor, powersupply, microsd #1 to raspberry pi Zero
8. Boot up Raspbian
9. Login with pi/raspberry.
10. Check hardware entropy with:
    a. cat /proc/random/entroy_avail => it should read in the 3000s (higher is better)
11. Use GPG to create a master key (4096 bit; some people use a weird number)
12. Create a revocation certificate.
13. Backup the revocation certificate and the master key on both USB. I may want to print it out by connecting a USB printer.
14. Put the backups in a safe place.
15. Create subkeys - size 2048
    i. signature
    ii. encryption
    iii. authentication
16. Backup the subkeys
17. 





References:  

1. https://blog.josefsson.org/2014/06/23/offline-gnupg-master-key-and-subkeys-on-yubikey-neo-smartcard/
