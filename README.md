# thcrap-steam-proton-wrapper
## Introduction
A wrapper script for launching the official Touhou games on Steam with the [Touhou Community Reliant Automatic Patcher](https://www.thpatch.net/) (thcrap), and optionally the [Vsync patch](https://en.touhouwiki.net/wiki/Game_Tools_and_Modifications#Vsync_Patches) (vpatch), from the Steam client using [Proton](<https://en.wikipedia.org/wiki/Proton_(software)>) on GNU/Linux.

This script works by setting up and managing a global thcrap instance, launching the configuration tool when needed, all without user intervention.

Before launching a game, this script takes the command Steam would normally run, and alters it to include the patch loader. This creates a seamless integration between the Steam client and thcrap, allowing the games to function as if they weren't patched at all!

### Rationale

On Windows, when launching a game bought on Steam with thcrap, Steam will be able to detect and automatically wrap it, so integration will work fine.

On Linux, this isn't the case. Due to the compatibility layer used to run the games, launching them from outside the Steam client will make Steam unable to detect that they are running. This means that, while using thcrap, Steam integration won't be available, your playtime won't be tracked, and your friends won't be able to see that you are playing weird indie Japanese shmups.

Also, with Steam Play/Proton, it is expected that you can run your games without having to mess around with Wine. For Touhou, it's annoying having to fire up Wine to be able to set-up the translation patches, and then having no proper integration with Steam when trying to play the games.

## How to use
[Here's a video tutorial](https://www.youtube.com/watch?v=6rZxeyILYmo) by [Maxmani](https://www.youtube.com/c/Maxmani).

### 1. Installation
#### AUR

    yay -S thcrap-steam-proton-wrapper-git

#### For Steam Flatpak

    flatpak install flathub com.valvesoftware.Steam.Utility.thcrap_steam_proton_wrapper

#### Manual installation

Download the script, mark it as executable, and put under `/usr/local/bin/` (or somewhere that you find convenient):

    curl -O https://raw.githubusercontent.com/tactikauan/thcrap-steam-proton-wrapper/master/thcrap_proton
    chmod +x thcrap_proton
    mv thcrap_proton /usr/local/bin

### 2. Editing the script (optional)
If you have gone through the manual installation, you have the opportunity to change the following variables inside the script:
- The `THCRAP_FOLDER` variable points to where the thcrap installation will be located. You can to change it, for example, if you want it to point to your current thcrap installation.
- The `THCRAP_CONFIG` defines the config file that will be loaded when no other is specified in the launch options.

### 3. Setting the launch options
Go to your Steam library -> right click the game -> Properties -> and edit the launch options to:

    thcrap_proton -- %command%
   
In case you have put your script outside `/usr/local/bin/`, you'll have to provide the full path:
   
    /path/to/thcrap_proton -- %command%

This is the base command, which will run the game with the default config.

To change the config file loaded by thcrap, use the `-c` flag.

To enable vpatch for that game, include the `-v` flag.
   
**Note that this script does not install vpatch on it's own.**

So, if I wanted to run the game with vpatch and Brazilian Portuguese translations, the command would look like this:
       
    thcrap_proton -v -c pt-br.js -- %command%

**Note: the `%command%` always comes at the end**

If you want to use any environment variables in your launch options, you can put them before the `%command%`, like this:
   
    thcrap_proton -- PROTON_USE_WINED3D=1 %command%

### 4. Running the game
Upon first launch, the script will download and set-up a thcrap instance, if there's not one already, and then launch the configuration tool, so you can generate your config files.

After that, when the thcrap loader window shows up, click on 'Settings and logs' and uncheck 'Keep the updater running in background', so Steam can correctly detect when the game is closed.

And thats it!

## Advanced
### Debugging
The script sends all it's output to `/tmp/thcrap_proton.log`, which includes the original and modified launch commands.
