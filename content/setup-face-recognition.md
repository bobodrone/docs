---
title: Setup facial recognition for authentication (howdy).
description:
  Want to forever forget about typing in a sudo password?
keywords:
  - howdy
  - login
  - utilities

facebookImage: /_social/article
twitterImage: /_social/article

hidden: false
section: community
tableOfContents: true
---

In this article we will setup [Howdy](https://github.com/Boltgolt/howdy) - open source face authentication tool.
Please make sure your webcam works before you start installation. An article about webcam troubleshooting can be found [here](/content/webcam.md).

## Installation on Pop!\_OS, Ubuntu or any other Ubuntu based distribution

Firstly, we will need a terminal. You can open it by pressing Super Key <kbd><font-awesome-icon :icon="['fab', 'ubuntu']"></font-awesome-icon></kbd>/<kbd><font-awesome-icon :icon="['fab', 'pop-os']"></font-awesome-icon></kbd> on your keyboard or searching for terminal app in your application menu.

Then, adding a Howdy PPA is needed so our system knows where to get the app:

```bash
sudo add-apt-repository -y ppa:boltgolt/howdy
```

Now that we added Howdy repository to your system we can proceed to installing howdy itself. For that you will need to execute one more terminal command:

```bash
sudo apt install -y howdy
```

## Installation on other distributions (Debian, Arch, Fedora, openSUSE)

Please refer to first party instructions [here](https://github.com/Boltgolt/howdy#installation).

## Configuring Howdy

During the configuration of Howdy, you will be asked "What profile would you like to use?". Eg:

```
Preparing to unpack .../106-howdy_2.6.1_all.deb ...

Starting certainty auto config...

After detection, Howdy knows how certain it is that the match is correct.
How certain Howdy needs to be before authenticating you can be customized.

F: Fast.
Allows more fuzzy matches, but speeds up the scanning process greatly.

B: Balanced.
Still relatively quick detection, but might not log you in when further away.

S: Secure.
The safest option, but will take much longer to authenticate you.

You can always change this setting in the config.
What profile would you like to use? [f/b/s]:
```

Select profile you want to use by typing <kbd>f</kbd>, <kbd>b</kbd>, <kbd>s</kbd> for Fast, Balanced or Secure profiles, accordingly, and pressing <kbd>Enter</kbd>.
After that Howdy will download and install required dependencies for face recognition.

When everything is done you will be returned to terminal. Now it's time to add a face for login. For that, run:

```bash
sudo howdy add
```

You will be asked for your sudo password, type it in and press <kbd>Enter</kbd>. You will be asked to label the new model. It's similar to how you're asked to label a fingerprint you're adding on your phone. Something like 'face1' or 'John's face' will suffice.

Once that's done, you successfully finished setting up howdy. Try it out by locking your screen and pressing any button to open login. If everything is setup correctly - you will be logged in by <u>Howdy</u> with your face.

## Troubleshooting

### "Camera path is not configured correctly, please edit the 'device_path' config value." error when adding a face

#### Finding out what webcam to use

Open a terminal window if you don't have one already and execute:

```bash
ls /dev | grep "video."
```

This will show you all video inputs you have on your system.
Now, we need to test which of them work for you.
For that, we can use `ffplay` command. Use it as such:

```bash
ffplay /dev/INPUT
```

Replace `INPUT` with results you had from previous command.

#### Configuring Howdy to use webcam you want

Once you find a webcam you want to use for Howdy, type in:

```bash
sudo howdy config
```

That will open a configuration file for Howdy in CLI editor called Nano.
Find a device path variable there, it should look like that:

```
device_path = /dev/v4l/by-path/none
```

Change whatever it equals to input you want, like that:

```
device_path = /dev/video0
```

After that's done, exit and save the config file by pressing <kbd>ctrl</kbd>+<kbd>X</kbd>, <kbd>Y</kbd> and then <kbd>enter</kbd>.
After you exit the config file and get back to terminal, try adding your face to Howdy again, with:

```bash
sudo howdy add
```

### Howdy always says "Timeout is reached" and never identifies you

You can try lowering certainty level of howdy--in by editing it's configuration file. Hit the Super Key <kbd><font-awesome-icon :icon="['fab', 'ubuntu']"></font-awesome-icon></kbd>/<kbd><font-awesome-icon :icon="['fab', 'pop-os']"></font-awesome-icon></kbd> and <kbd>T</kbd> to open a terminal window.
Once you get there, type in:

```bash
sudo howdy config
```

and press <kbd>enter</kbd>. That will open a configuration file for Howdy in CLI editor called Nano.
Find an option called certainty level. It should look something like that:

```
certainty = 2.8
```

To make howdy more forgiving, rise that number a bit, to 3.8, for example. Be aware, values over 5 are not reccommended.
When that's done, exit and save the config file by pressing <kbd>ctrl</kbd>+<kbd>X</kbd>, <kbd>Y</kbd> and then <kbd>enter</kbd>.
Now Howdy is more likely to identify you in different conditions.

### Removing a saved face print

You can delete existing face print you added. For that, you will need to get a list of all prints you got:

```bash
sudo howdy list
```

You might be requested to enter your sudo password and press <kbd>enter</kbd>.
After you decide what print to delete, memorize it's ID. A number on the left.
Now all you need to delete it from Howdy is to execute this little command in the terminal:

```bash
sudo howdy remove ID
```

But replacing ID with number you memorized a step before.

### Uninstalling Howdy

If you, for some reason, want to uninstall Howdy.
Open a terminal window by pressing the Super Key <kbd><font-awesome-icon :icon="['fab', 'ubuntu']"></font-awesome-icon></kbd>/<kbd><font-awesome-icon :icon="['fab', 'pop-os']"></font-awesome-icon></kbd> and <kbd>T</kbd> or searching for a Terminal app in your applications menu. Type in:

```bash
sudo howdy clear
```

And press enter to delete all face prints. Entering sudo password may be required.
Then, execute:

```bash
sudo apt purge howdy
```

To delete Howdy itself.
And lastly, remove Howdy PPA repository:

```bash
sudo add-apt-repository ppa:boltgolt/howdy --remove
```

After that's done, there should be nothing left of Howdy on your computer.

---

This article was contributed by [Vega](https://github.com/smth-0).
