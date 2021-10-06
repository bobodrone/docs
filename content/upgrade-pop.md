---
title: Upgrade Pop!_OS
description: >
  Get the newest version of Pop!_OS on your computer! Check out our upgrade directions.
keywords:
  - Pop!_OS 17.10
  - Pop!_OS 18.04
  - Pop!_OS 18.10
  - Pop!_OS 19.04
  - Pop!_OS 19.10
  - Pop!_OS 20.04
  - Pop!_OS 20.10
  - Pop!_OS 21.04
  - LTS
  - Non-LTS
  - Upgrade
  - Update
  - Release
  - System76
  - apt

facebookImage: /_social/article
twitterImage: /_social/article

hidden: false
section: software
tableOfContents: true
---

## When Should I Upgrade?

Before upgrading, it can be helpful to ask yourself the following environment questions:

1. **Do I have a backup of my data (see next section).**

    - YES? - Proceed.
    - NO? - Make a Backup.

2. **Is this a mission critical machine?**

    - YES? - Wait until upgrading will not stop work.
    - NO? - Proceed.

3. **Do I have a backup/alternate machine if this one goes down temporarily?**

    - YES? - Proceed.
    - NO? - Make a backup, assess your comfort level, proceed if comfortable.

4. **How recently was this upgrade issued? Have any stability issues been reported?**

    - YES? - Wait a week (or two), check again, then upgrade.
    - NO? - Proceed.

## Backing Up Your Data

**IMPORTANT NOTE:** The upgrade process will leave your files in place, but no matter which system version you are running, we **always** recommend first creating a good backup of your files, just to be on the safe side should anything unexpected happen during the upgrade.

Please read our article on [how to backup your files](/articles/backup-files/) for helpful instructions.

## Upgrade Pop!_OS

Pop!\_OS 21.04 was released on June 29, 2021.

Pop!\_OS 20.10 was released October 23, 2020.

### Upgrading Pop!\_OS to 21.04 from 20.10

> **NOTE:** For all other operating system versions scroll down to the instructions for upgrading from an earlier release

First, make sure you have applied all updates to your system. You can do this through the Pop!\_Shop, or through the terminal:

```bash
sudo apt update
sudo apt full-upgrade
```

Once the updates are applied, a notification should appear at the top of your screen saying that an upgrade is available. Click on this notification, or go to Settings ->  OS Upgrade & Recovery. The System76 upgrade package will display a message that Pop!\_OS 21.04 is available with a **Download** button.

![Settings OS Upgrade](/images/upgrade-pop/setting-os-upgrade.png)

1. Click the **Update** button to update the Recovery partition.

2. Click the **Download** button and the download will begin. Once the download is complete, you will receive a second notification saying the upgrade is ready.

3. Click on the notification and your computer will restart to the upgrade screen.

After the upgrade is finished, you will be taken back to the login page, and voila! Your system is now running Pop!\_OS 21.04!

### Advanced Install (Terminal)

Use the Terminal to apply the upgrade may do so by running the following commands:

```bash
sudo apt update
```

You'll be prompted to enter your system password, but when you type it, the letters won't show. Just continue typing the password and press <kbd>Enter</kbd>.

```bash
sudo apt full-upgrade
```

Update the Recovery partition with this command:

```
pop-upgrade recovery upgrade from-release
```

Once updates have been applied (including the Recovery partition), initiate the upgrade with this command:

```bash
pop-upgrade release upgrade
```

Please check the terminal window at different times during the update process to make sure you answer any prompts asking you to type <kbd>Y</kbd> or <kbd>Enter</kbd>. Some of the prompts to expect are described below.

If you have 3rd party sources enabled, you will be prompted about the sources being disabled during the upgrade. Press <kbd>Enter</kbd> to continue.

Type <kbd>Y</kbd> and press enter when prompted about starting the upgrade.

If you have your lock screen set to enabled, you will receive a prompt about the screen being disabled during the update. Press <kbd>Enter</kbd> to continue.

Near the end of the upgrade process, you'll be prompted to remove obsolete packages. Type <kbd>Y</kbd> and press <kbd>Enter</kbd>  when prompted.

You may receive a notice about the keyboard layout and the option to use the package maintainer's version of a certain package. If you haven't specifically made a change to a configuration file in your system, go ahead and press <kbd>Y</kbd> to use the package maintainer's version. If you have made a change you would like to keep, press <kbd>N</kbd> to use the local version, or press <kbd>D</kbd> to inspect the changes and see which version you would like to use.

Finally, you will get a notice to restart your computer to complete the upgrade. Make sure all files are saved and any open programs are closed. You can reboot via the terminal by typing `[sudo] reboot` and pressing <kbd>Enter</kbd>. If you prefer, you can also use the graphical interface to reboot.

---

Once restarted, the computer will be on the newly upgraded system! If you run into any issues, check out our troubleshooting section below.

## Upgrading older releases

Upgrading Pop!\_OS 17.10 (artful) 18.10 (cosmic), 19.04 (disco) or 19.10 (eoan) will require upgrading to Pop!\_OS 20.04 (focal) LTS before upgrading to the current Pop!\_OS 21.04 (hirsute).

These older Pop!\_OS releases are now unsupported and no new updates are available. After unsupported versions have been removed from the archive and mirror network, you will need to change where your system checks for un-applied updates to be able to upgrade. Open a terminal and follow the next set of instructions to upgrade from Pop!\_OS 18.10, 19.04, or 19.10.

### 1. Get your current system fully updated

```bash
sudo sed -i 's/us.archive/old-releases/g' /etc/apt/sources.list
sudo apt update -m
sudo dpkg --configure -a
sudo apt install -f
sudo apt full-upgrade
sudo apt install pop-desktop
```

### 2. Move any PPA additions out of the way, and get all of the sources pointed at the 20.04 versions

```bash
sudo mkdir -p /etc/apt/backup
sudo mv /etc/apt/sources.list.d/* /etc/apt/backup
sudo apt-add-repository -yn ppa:system76-dev/stable
sudo apt-add-repository -yn ppa:system76/pop
sudo sed -i 's/old-releases/us.archive/g' /etc/apt/sources.list
sudo sed -Ei 's/cosmic|eoan|disco/focal/g' /etc/apt/sources.list /etc/apt/sources.list.d/*.list
```

**Note:** to change 'disco' to your release that you are trying to update from

### 3. Now, do the upgrade

This will have a three phases and once the download is complete, you can't change your mind. This works from a new install of 19.04, but some packages may add complications, so make sure you have a backup of important data before moving forward. There may be a question about restarting services, and it is safe to answer "Yes". There may be some questions about using "maintainer" version of configuration files and using the that new version is also very likely what you want to do (and the default answer will work):

```bash
sudo apt update
sudo apt install dpkg apt
sudo apt full-upgrade | tee ~/upgrade.log
```

### 4. Now put the PPAs back

You will want to take a look at the files that end in "list" in "/etc/apt/backup" to see if you want to enable that again by moving them back to the /etc/apt/sources.list.d/ directory.

### 5. After the 20.04 Pop upgrade is complete, **reboot**

### 6. Run the command to upgrade to Pop 21.04

```bash
pop-upgrade release upgrade
```

## Troubleshooting

Most upgrades proceed without a hitch, but occasionally things go wrong. If your system hangs on the upgrade process or if it reboots and won't start, then the following steps can help repair your broken OS.

### Stuck Upgrades

If your upgrade appears to hang in place for an extended period of time, click on the **Terminal** item to expand the terminal section. Check what action is available there, then complete the steps to unhang your upgrade. For help, contact support and we'll be able to provide assistance.

### Broken Upgrade

If the upgrade fails it will most likely be due to a package manager issue.  First, run an alternative upgrade manager with this command, and see if it will manage further:

```bash
do-release-upgrade
```

If it fails again, the package manager will need to be repaired manually.  Please follow the steps as outlined in this document about [repairing the package manager](/articles/package-manager-pop/)

Make sure to get the package manager to a fully upgraded status before rebooting your computer.  You should see this line after running all 6 of the repair package manager commands:

> 0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
