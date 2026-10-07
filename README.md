# Peer Pressure Softmod For Xbox 360
Peer Pressure is a persistent softmod exploit for Xbox 360. It works by making some modifications to the console NAND image and HDD security sectors to trigger a series of DMA attacks during startup which ultimately allow you to run unsigned code. Here's some of the features that Peer Pressure provides, for a complete list see [Softmod Features](../../wiki/Softmod-Features):
- Removes all code signing restrictions on executables files allowing you to run homebrew applications.
- Removes licensing and security checks for games, downloadable content, arcade games, indie games, etc.
- Includes 3 different modes to boot your console allowing you to easily switch between a hacked and retail state.
- Integrated softmod settings application with auto update functionality that'll alert you when a new update is available.
- Flash (NAND) write protection to prevent accidental modifications to your console's NAND image which could prevent your console from booting.
- Boot xell (linux, etc.) on startup by powering the console on via the eject button.
- Sideload DLLs into games/apps/etc. which can be used for things like loading [RB3Enhanced](https://github.com/RBEnhanced/RB3Enhanced), or allowing older dashboards (blades, NXE) to be used.
- Boot animation editor that allows for customizing the blades era boot animation.

Note: Peer Pressure does not support dashlaunch or dashlaunch plugins, for more information on why this is see [Homebrew Comptability](#Homebrew-Compatibility).

# Disclaimer
This softmod exploit will make modifications to your console's NAND image and HDD security sectors which could cause data loss and/or prevent your console from booting. While I have done my best to ensure issues like this should never happen there's always a chance that something could go wrong and require a hardware NAND flasher to recover from. By using this software you are doing so at your own risk and I, Grimdoomer, am in no way responsible for any damage or data loss that may occur as a result of using it.

# Requirements
To use the Peer Pressure softmod you'll need the following:
- An Xbox 360 console.
- A mechanical HDD.
- USB stick.

## Console Compatibility
The peer pressure exploit does not work on all console revisions, specifically the corona and winchester models cannot be used. The following table shows which console revisions are supported by the exploit:
| Motherboard Revision | Supported |
| -------------------- | --------- |
| Xenon | ✔ |
| Zephyr | ✔ |
| Falcon | ✔ |
| Jasper | ✔ |
| Trinity | ✔ |
| Corona | ❌ Southbridge has a hardware level fix for the exploit. |
| Winchester | ❌ Southbridge has a hardware level fix for the exploit. |

## HDD/SSD Compatibility
To use this exploit you will need a hard drive, it doesn't need to be an official Xbox 360 hard drive, or even formatted, you just need **any** mechanical 2.5" HDD that can be connected to the console. The exploit works by attacking a race condition that exists when reading the security sectors off the HDD. Due to this being timing critical most SSDs will likely not work with the exploit as they respond to read requests faster than the race condition can be exploited. 

During beta testing it was found that * *some* * SSDs will also work with the exploit, with the common factor being ones with DRAM cache were more likely to work with the exploit. You can try to use the exploit with a SSD if you choose, however, for optimal boot times and reliability it's recommended to use a mechanical HDD instead.

## How To Install
For detailed steps on how to install the Peer Pressure softmod please see the [Installation](../../wiki/Installation) page in the wiki.

## Boot Times
When everything is working correctly the expected boot times should be about ~5 seconds longer than the boot times of an unmodified console. Given that this exploit is based on a race condition if the attack isn't successful the SMC will automatically reboot the console to try again, adding additional time to bootup. However, the exploit typically triggers on the first attempt (results will vary if using a SSD) so this should be an edge case and not the norm.

Two factors that'll increase the boot times are: how long the HDD takes to spin up and the size of the HDD. When the console is powered on the HDD doesn't spin up until the console tries to communicate with it, and the exploit can't begin until that happens. The longer it takes for the HDD to spin up, the longer the boot times will be. By making a [small modification to the HDD SATA adapter](../../wiki/Quick-Boot-SATA-Mod) you can force the drive to spin up as soon as power is applied rather than waiting for the console to communicate with it. This will shave off ~2-3 seconds from the boot time and is what I personally use on my console. 

However, the mod requires precision soldering so if you aren't able to do something like the RGH install with ease you most likely won't be able to perform this mod either. Despite using this on my own console I wouldn't recommend it unless you really want to speed run the boot times.

When the exploit triggers the HDD needs to be mounted so the file system can be accessed. The time it takes to mount a FATX partition increases linearly with the size of the partition, meaning, the larger the HDD, the longer it takes to mount, the longer the console takes to boot. You can tell how long this takes by how long the console LED's stay on the "stage 2" indicator (top 2 LEDs are illuminated). 

# Homebrew Compatibility
While Peer Pressure will allow you to use homebrew games and applications there are some homebrew applications that aren't supported, namely dashlaunch and dashlaunch "plugins". The reason for this is that dashlaunch plugins all use fixed memory addresses and perform hooks into the Xbox 360 OS (kernel, xam, etc.). This means they're not only competing for the same fixed address space but are also competing to hook the same functions in the Xbox 360 OS. This is why most dashlaunch plugins are incompatible with each other and cannot be loaded at the same time (also why there's a load order).

When I decided to make Peer Pressure **I wanted a clean slate to make my own thing** and didn't want to be held back by janky dashlaunch plugins that haven't been updated in 10+ years. Peer Pressure already includes most of the dashlaunch functionality with the exception of loading plugins. There's nothing stopping you from force loading whatever dlls you want, but they'll likely have stability issues and cause built-in softmod features to stop working. For these reasons dashlaunch plugins are strictly not supported, and any bug reports filed while using them will be closed without investigation.

**If this bothers you you don't have to use it.**

For some of the features people currently use dashlaunch plugins for (xbdm, HID controller support, etc.) I would like to add these as mainline features in the future. However, this all depends on time and motivation. For a list of features that are currently planned for the next release see [Feature Requests/Roadmap](../../issues/1).

# Where's the Source Code?
I have decided not to release the source code for this for a few reasons:
1. Everything I make and release I do for free in my own time. This is a hobby for me and something I do for fun. Given the state of software development I no longer feel the need to release the source code for everything I make just so people can make a million forks of it and kill any motivation I have to work on it. **I make this stuff for myself and not anyone else.**
2. The world doesn't need 15 "slopsoftmods". It took a LOT of work to not only develop this exploit chain but also package it into an easy to use installer and do testing to try and make sure it wouldn't break someone's console. Making these modifications to your console can cause it to stop working, and I don't see why someone who doesn't understand the risks, work it took to make, or how the OS functions, should be able to clone a turn-key repository, make one change, slap their name on it, and tell people "go download my softmod".

It's inevitable that people will just reverse engineer, modify, and repackage my work as their own, so for anyone who's thinking about using such things heed this warning: There's real risk installing these types of modifications on your console. While my work may not be "perfect, bug free, etc.", I have taken utmost care to make sure it's unlikely to cause any hard to your console and that you should always be able to boot into retail mode to recover from anything that goes wrong. Other people copying my work may not do the same. If you don't trust the people making the modifications don't install them.

# Credits

