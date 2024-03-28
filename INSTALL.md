# Instructions

These instructions explain how to set up the tools required to build **pokeemerald_pc**, which assembles the source files into an EXE or ROM.

These instructions come with notes which can be expanded by clicking the "<i>Note...</i>" text.
In general, you should not need to open these unless if you get an error or if you need additional clarification.

If you run into trouble, ask for help on Discord or IRC (see [README.md](README.md)).

- [Windows 10/11](#windows-1011-wsl2)
- [MacOS](#macos)
- [Linux](#linux)

## Windows 10/11 (WSL2)
WSL2 is the preferred terminal to build **pokeemerald_pc**. The following instructions will explain how to install WSL2 (referred to interchangeably as WSL).
- If WSL (Debian or Ubuntu) is **not installed**, then go to [Installing WSL2](#Installing-WSL2).
- Otherwise, if WSL is installed, but it **hasn't previously been set up for another decompilation project**, then go to [Setting up WSL2](#Setting-up-WSL2).
- Otherwise, **open WSL** and go to [Choosing where to store pokeemerald_pc (WSL2)](#Choosing-where-to-store-pokeemerald_pc-WSL2).

### Installing WSL2
1. Open [Windows Powershell **as Administrator**](https://i.imgur.com/QKmVbP9.png), and run the following command (Right Click or Shift+Insert is paste in the Powershell).

    ```powershell
    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
    ```

2. Once the process finishes, restart your machine.

3. The next step is to choose and install a Linux distribution from the Microsoft Store. The following instructions will assume Ubuntu as the Linux distribution of choice.
    <details>
        <summary><i>Note for advanced users...</i></summary>

    >   You can pick a preferred Linux distribution, but setup instructions may differ. Debian should work with the given instructions, but has not been tested.
    </details>

4. Open the [Microsoft Store Linux Selection](https://aka.ms/wslstore), click Ubuntu, then click Get, which will install the Ubuntu distribution.
    <details>
        <summary><i>Notes...</i></summary>

    >   Note 1: If a dialog pops up asking for you to sign into a Microsoft Account, then just close the dialog.
    >   Note 2: If the link does not work, then open the Microsoft Store manually, and search for the Ubuntu app (choose the one with no version number).
    </details>

### Setting up WSL2
Some tips before proceeding:
- In WSL, Copy and Paste is either done via
    - **right-click** (selection + right click to Copy, right click with no selection to Paste)
    - **Ctrl+Shift+C/Ctrl+Shift+V** (enabled by right-clicking the title bar, going to Properties, then checking the checkbox next to "Use Ctrl+Shift+C/V as Copy/Paste").
- Some of the commands that you'll run will ask for your WSL password and/or confirmation to perform the stated action. This is to be expected, just enter your WSL password and/or the yes action when necessary.

1. Open **Ubuntu** (e.g. using Search).
2. WSL/Ubuntu will set up its own installation when it runs for the first time. Once WSL/Ubuntu finishes installing, it will ask for a username and password (to be input in).
    <details>
        <summary><i>Note...</i></summary>

    >   When typing in the password, there will be no visible response, but the terminal will still read in input.
    </details>

3. Update WSL/Ubuntu before continuing. Do this by running the following command. These commands will likely take a long time to finish:

    ```bash
    sudo apt update && sudo apt upgrade
    ```

4. Certain packages are required to build pokeemerald_pc. Install these packages by running the following command:

    ```bash
    sudo apt install build-essential binutils-arm-none-eabi git libpng-dev gdebi-core make g++-mingw-w64-i686
    ```
    <details>
        <summary><i>Note...</i></summary>

    >   If the above command does not work, try the above command but replacing `apt` with `apt-get`.
    </details>

5. Once `gdebi-core` is done installing, download the devkitPro pacman package [here](https://github.com/devkitPro/pacman/releases). The file to download is `devkitpro-pacman.amd64.deb`.

6. Change directory to where the package was downloaded. For example, if the package file was saved to **C:\Users\\_\<user>_\Downloads** (the Downloads location for most users), enter this command, where *\<user> is your **Windows** username:

    ```bash
    cd /mnt/c/Users/<user>/Downloads
    ```

7. Once the directory has been changed to the folder containing the devkitPro pacman package, run the following commands to install devkitARM.

    ```bash
    sudo gdebi devkitpro-pacman.amd64.deb
    sudo dkp-pacman -Sy
    sudo dkp-pacman -S gba-dev
    ```
    The last command will ask for the selection of packages to install. Just press Enter to install all of them, followed by entering Y to proceed with the installation.

    <details>
        <summary><i>Note...</i></summary>

    > Note: `devkitpro-pacman.amd64.deb` is the expected filename of the devkitPro package downloaded (for the first command). If the downloaded package filename differs, then use that filename instead.
    </details>

8. Run the following command to set devkitPro related environment variables (alternatively, close and re-open WSL):

    ```bash
    source /etc/profile.d/devkit-env.sh
    ```

### Choosing where to store pokeemerald_pc (WSL2)
WSL has its own file system. Windows files *are* accessible from WSL, but accessing Windows files using WSL2 is incredibly slow, so you want to store your files in the WSL filesystem.

We'll make a `decomps` folder for organisation:

```bash
mkdir decomps && cd decomps
```

<details>
    <summary><i>Notes...</i></summary>

>   Note 1: The WSL file system appears in Windows similar to network folders. You can find 'Linux' at the bottom of your Quick Access menu.
>   Note 2: It is suggested that you go into your file explorer in Windows, find the WSL file system and pin your decomps folder for easier access.
</details>

If this works, then proceed to [Installation](#installation).

## macOS
1. If the Xcode Command Line Tools are not installed, download the tools [here](https://developer.apple.com/xcode/resources/), open your Terminal, and run the following command:

    ```bash
    xcode-select --install
    ```

2.  - If libpng is **not installed**, then go to [Installing libpng (macOS)](#installing-libpng-macos).
    - If devkitARM is **not installed**, then go to [Installing devkitARM (macOS)](#installing-devkitarm-macos).
    - Otherwise, **open the Terminal** and go to [Choosing where to store pokeemerald_pc (macOS)](#choosing-where-to-store-pokeemerald_pc-macos)

### Installing libpng (macOS)
<details>
    <summary><i>Note for advanced users...</i></summary>

>   This guide installs libpng via Homebrew as it is the easiest method, however advanced users can install libpng through other means if they so desire.
</details>

1. Open the Terminal.
2. If Homebrew is not installed, then install [Homebrew](https://brew.sh/) by following the instructions on the website.
3. Run the following command to install libpng.

    ```bash
    brew install libpng
    ```
    libpng is now installed.

    Continue to [Installing devkitARM (macOS)](#installing-devkitarm-macos) if **devkitARM is not installed**, otherwise, go to [Choosing where to store pokeemerald_pc (macOS)](#choosing-where-to-store-pokeemerald_pc-macos).

### Installing devkitARM (macOS)
1. Download the `devkitpro-pacman-installer.pkg` package from [here](https://github.com/devkitPro/pacman/releases).
2. Open the package to install devkitPro pacman.
3. In the Terminal, run the following commands to install devkitARM:

    ```bash
    sudo dkp-pacman -Sy
    sudo dkp-pacman -S gba-dev
    sudo dkp-pacman -S devkitarm-rules
    ```

    The command with gba-dev will ask for the selection of packages to install. Just press Enter to install all of them, followed by entering Y to proceed with the installation.

4. After the tools are installed, devkitARM must now be made accessible from anywhere by the system. To do so, run the following commands:

    ```bash
    export DEVKITPRO=/opt/devkitpro
    echo "export DEVKITPRO=$DEVKITPRO" >> ~/.bashrc
    export DEVKITARM=$DEVKITPRO/devkitARM
    echo "export DEVKITARM=$DEVKITARM" >> ~/.bashrc

    echo "if [ -f ~/.bashrc ]; then . ~/.bashrc; fi" >> ~/.bash_profile
    ```

### Installing pkg-config (macOS)
1. Open the terminal.
2. Run the following command to install pkg-config:

    ```bash
    brew install pkg-config
    ```

### Choosing where to store pokeemerald_pc (macOS)
At this point, you can choose a folder to store pokeemerald_pc into. If you're okay with storing pokeemerald_pc in the user folder, then proceed to [Installation](#installation). Otherwise, you'll need to account for where pokeemerald_pc is stored when changing directory to the pokeemerald_pc folder.

For example, if you want to store pokeemerald_pc (and agbcc) in **~/Desktop/decomps**, enter this command to **change directory** to the desired folder:
```bash
cd Desktop/decomps
```
Note that the directory **must exist** in the folder system. If you want to store pokeemerald_pc in a dedicated folder that doesn't exist (e.g. the example provided above), then create the folder (e.g. using Finder) before executing the `cd` command.

<details>
    <summary><i>Note..</i>.</summary>

>   Note: If the path has spaces, then the path must be wrapped with quotations, e.g. `cd "Desktop/decomp folder"`
</details>

If this works, then proceed to [Installation](#installation).

## Linux
Open Terminal and enter the following commands, depending on which distro you're using.

### Debian/Ubuntu-based distributions
1. Run the following command to install the necessary packages:
    ```bash
    sudo apt install build-essential binutils-arm-none-eabi git libpng-dev gdebi-core make g++-mingw-w64-i686
    ```

2. Download the devkitPro pacman package [here](https://github.com/devkitPro/pacman/releases). The file to download is `devkitpro-pacman.amd64.deb`.

3. Change directory to where the package was downloaded. Then, run the following commands to install devkitARM:

    ```bash
    sudo gdebi devkitpro-pacman.amd64.deb
    sudo dkp-pacman -Sy
    sudo dkp-pacman -S gba-dev
    ```
    The last command will ask for the selection of packages to install. Just press Enter to install all of them, followed by entering Y to proceed with the installation.

    > Note: `devkitpro-pacman.amd64.deb` is the expected filename of the devkitPro package downloaded (for the first command). If the downloaded package filename differs, then use that filename instead.

4. Run the following command to set devkitPro related environment variables (alternatively, close and re-open the Terminal):

    ```bash
    source /etc/profile.d/devkit-env.sh
    ```

Then proceed to [Choosing where to store pokeemerald_pc (Linux)](#choosing-where-to-store-pokeemerald_pc-linux).

### Other distributions
_(Specific instructions for other distributions would be greatly appreciated!)_

1. Try to find the required software in its repositories:
    - `gcc`
    - `g++`
    - `make`
    - `git`
    - `libpng-dev`
    - `gdebi-core`
    - `make` 
    - `g++-mingw-w64-i686`

2. Follow the instructions [here](https://devkitpro.org/wiki/devkitPro_pacman) to install devkitPro pacman. As a reminder, the goal is to configure an existing pacman installation to recognize devkitPro's repositories.
3. Once devkitPro pacman is configured, run the following commands:

    ```bash
    sudo pacman -Sy
    sudo pacman -S gba-dev
    ```

    The last command will ask for the selection of packages to install. Just press Enter to install all of them, followed by entering Y to proceed with the installation.

### Choosing where to store pokeemerald_pc (Linux)
At this point, you can choose a folder to store pokeemerald_pc into. If so, you'll have to account for the modified folder path when changing directory to the pokeemerald_pc folder.

If this works, then proceed to [Installation](#installation).

## Installation

If pokeemerald_pc is not already downloaded (some users may prefer to download pokeemerald_pc via a git client like GitHub Desktop), run:

```bash
git clone https://github.com/Kurausukun/pokeemerald -b pc_port pokeemerald_pc
```

<details>
    <summary><i>Note for WSL2...</i></summary>

>   If you get an error stating `fatal: could not set 'core.filemode' to 'false'`, then run the following commands:
>   ```bash
>   cd
>   sudo umount /mnt/c
>   sudo mount -t drvfs C: /mnt/c -o metadata,noatime
>   cd <folder where pokeemerald_pc is to be stored>
>   ```
>   Where *\<folder where pokeemerald_pc is to be stored>* is the path of the folder [where you chose to store pokeemerald_pc](#Choosing-where-to-store-pokeemerald_pc-WSL2). Then run the `git clone` command again.
</details>

Now move on to [downloading the SDL Libraries](#sdl_libraries).

## SDL Libraries

1. Download SDL from: https://github.com/libsdl-org/SDL/releases. You need the Development Libraries for MinGW specifically, and the Runtime Library for 32-bit Windows as well.
    <details>
        <summary><i>Note for Releases</i></summary>

    >   You can download any recent version of SDL2.
    >   The Development Libaries for MinGW will be called SDL2-devel-x.xx.x-mingw.tar.gz
    >   The Runtime Libary will be called SDL2-x.xx.x-win32-x86.zip
    </details>

2. Uncompress the Development libraries, then move the SDL2-x.xx.x folder into your **pokeemerald_pc** folder.

3. Uncompress the Runtime libraries, then move the SDL2.dll file into your **pokeemerald_pc** folder.

4. Use your preferred text editor to adjust the path to SDL in **pokeemerald_pc**'s `Makefile_pc` if needed, like so:

> SDL_DIR := $(CURDIR)/SDL2-x.xx.x/i686-w64-mingw32

Where SDL2-x.xx.x is your exact version of SDL2.

Finally, you are ready to [build **pokeemerald_pc**](#build-pokeemerald_pc)

## Build pokeemerald_pc
If you aren't in the pokeemerald_pc directory already, then **change directory** to the pokeemerald_pc folder:
```bash
cd pokeemerald_pc
```

To build **pokeemerald_pc.exe**:
```bash
make pc
```
If it has built successfully you will have the output file **pokeemerald_pc.exe** in your project folder.

To build **pokeemerald_pc.gba**:
```bash
make
```
If it has built successfully you will have the output file **pokeemerald_pc.gba** in your project folder.

<details>
    <summary><i>Note for -jN...</i></summary>

>   To speed up the build time add -jN to your command.
>   e.g., `make -f Makefile_pc -jN`
>   
>   "N" stands for the number of CPU Threads you want to assign to the compiler.
>   The more threads, the faster it'll go.
>
>   Use command `nproc` to get the total core count of your system.
>   `nproc` is not available on macOS. The alternative is `sysctl -n hw.ncpu`.
</details>

# Building guidance

## Compare ROM to the original

For contributing, or if you'd simply like to verify that your ROM is identical to the original game, run:
```bash
make compare
```
If it matches, you will see the following at the end of the output:
```bash
pokeemerald_pc.gba: OK
```
If there are any changes from the original game, you will instead see:
```bash
pokeemerald_pc.gba: FAILED
shasum: WARNING: 1 computed checksum did NOT match
```

### Other toolchains

To build using a toolchain other than devkitARM, override the `TOOLCHAIN` environment variable with the path to your toolchain, which must contain the subdirectory `bin`.
```bash
make TOOLCHAIN="/path/to/toolchain/here"
```
The following is an example:
```bash
make TOOLCHAIN="/usr/local/arm-none-eabi"
```
To compile the `modern` target with this toolchain, the subdirectories `lib`, `include`, and `arm-none-eabi` must also be present.

# Useful additional tools

* [porymap](https://github.com/huderlem/porymap) for viewing and editing maps
* [poryscript](https://github.com/huderlem/poryscript) for scripting ([VS Code extension](https://marketplace.visualstudio.com/items?itemName=karathan.poryscript))
* [Tilemap Studio](https://github.com/Rangi42/tilemap-studio) for viewing and editing tilemaps
