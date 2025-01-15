# How to compile [Dr. Robotnik's Ring Racers](https://www.kartkrew.org/) for Windows

> <small>:information_source: This guide is intended for Windows 10 x64, but should also apply for Windows 11.</small>

The following programs are required to proceed:
- [Git for Windows](https://git-scm.com/)
- [vcpkg](https://vcpkg.io/en/)
- [MSYS2](https://www.msys2.org/)

If these programs aren't installed, start with the **Prerequisites** section.\
Otherwise, proceed directly to the **Compiling** section.

---

<video src="/assets/vcpkg-install-video.mp4" controls>

<details>
    <summary>
        <h3 style="margin-top: 0 !important; margin-bottom: 0 !important">Prerequisites</h3>
    </summary>

### 1. Install _Git_

- Download [**Git for Windows**](https://git-scm.com/downloads/win).
    > <small>32-bit or 64-bit, depending on [**your operating system**](https://support.microsoft.com/en-gb/windows/32-bit-and-64-bit-windows-frequently-asked-questions-c6ca9541-8dce-4d48-0415-94a3faa2e13d).</small>

- Follow each step in the setup wizard _as instructed_.
    > Leave each option set to its default value.
- Check if **Git** has been installed by opening your favourite terminal, and running `git --version`.  If successful, you should see something like this in your terminal:

    <img src="./git-version-verify.png">

### 2. Install _vcpckg_

- Clone the **vcpkg** repository into a folder of your choice using **Git**, by running the following command in your terminal:

    ```bash
     git clone https://github.com/microsoft/vcpkg.git
    ```

    By default, your terminal _should_ open in your home folder, so the full path of the repository would be:

    ```powershell
    C:/Users/<your username>/vcpkg
    ```

- Navigate to the newly-created `vcpkg` folder and run the bootstrap script:

    ```bash
    cd vcpkg; .\bootstrap-vcpkg.bat
    ```

- Check if **vcpkg** has been installed by running the following command:

    ```bash
    ./vcpkg --version
    ```

    ---
    <small>If you're unsure, refer to the following video:</small>
    > <details><summary>Installing <strong>vcpkg</strong></summary><video src="./vcpkg-install-video.mp4" controls></details>

### 3. Install _MSYS2_

- Download [the MSYS2 installer](https://www.msys2.org/).

- Follow each step in the setup wizard _as instructed._
    > Leave each option set to its default value.

- Once installation completes, a terminal window will open. This confirms that **MSYS2** was installed succesfully.

    <img src="./msys-terminal-window.png">  

    You can go ahead and close this window.

    ---
    <small>If you're unsure, refer to the following video:</small>
    > <details><summary>Installing <strong>MSYS2</strong></summary><video src="./msys-install-video.mp4" controls></details>
</details>

<details>
    <summary>
        <h3 style="margin-top: 0 !important; margin-bottom: 1!important">Compiling</h3>
    </summary>

### 1. Opening _MSYS2_

- Navigate to the default installation folder for MSYS2 (`C:/msys64`)

- Open the **MINGW32** shell (`mingw32.exe`), as denoted by the <img src="./mingw32-icon.png" width="20" height="20" style="vertical-align: middle"> icon.

    <img src="./mingw32-shell.png">

### 2. Updating the package database

- Update the package database and all installed packages by running the following command in the shell:

    ```bash
    pacman -Syu
    ```

    <img src="./mingw32-update-packages.png">

- When prompted with **_Proceed with installation?_**, type `Y` in the terminal and press `Enter`.

- After updating, you _might_ see a message prompting you to **close** the terminal window, like this:

    ```
    To complete this update all MSYS2 processes including this terminal will be closed. Confirm to proceed [Y/n]
    ```
    If you _don't_, continue to the [next step](#3-installing-the-required-packages).

    If you _do_, type `Y` in the terminal and press `Enter`.\
    To open the terminal window again, follow the instructions in [Step 1](#1-opening-msys2).

### 3. Installing the required packages
- In the **MINGW32** shell, execute the following command to install all the required packages:

    ```bash
    pacman -S make git mingw-w64-i686-gcc mingw-w64-i686-ninja mingw-w64-i686-cmake
    ```

    <img src="./mingw32-install-required-packages.png">

- When prompted with **_Proceed with installation?_**, type `Y` in the terminal and press `Enter`.

- To verify that all the required packages have been installed sucessfully, run the follow commands in the terminal:

    ```bash
    which ninja; which make; which cmake; which gcc; which g++; which git
    ```

    If the packages _have_ been installed succesfully, each command will return the path to its respective executable:

    <img src="./mingw32-verify-installed-packages.png">

### 4. Downloading the game's source code

> <small>:information_source: For demonstration purposes, this guide will use the _latest_ version of Dr. Robotnik's Ring Racers. As of **January 1st 2025**, that is <strong>[v2.3](https://github.com/KartKrewDev/RingRacers/tree/v2.3).</strong> </small>

- Clone the repository for _Ring Racers_, with the following command:

    ```bash
    git clone https://github.com/KartKrewDev/RingRacers.git RingRacersRepo
    ```

    This will create a new folder named `RingRacersRepo`, which will contain the game's source code.

    ---
    <small>If you're unsure, refer to the following video:</small>
    > <details><summary>Cloning the repository</summary><video src="./git-clone-rr.mp4" controls></details>

- Navigate to the new `RingRacersRepo` folder by running the command:

    ```bash
    cd RingRacersRepo
    ```
- Switch the current branch to **v2.3** with the following command:
    ```bash
    git checkout v2.3
    ```

- Verify that your branch is set to **v2.3**, by running:

    ```bash
    git branch
    ```

    You will see an asterisk (*) next to the current branch, which should say `(HEAD detached at v2.3)`.

    <img src="./git-verify-branch.png">



### 5. Configuring the game for compilation
- Set the `VCPKG_ROOT` environment variable by running the following:<sup>:star:</sup>

    ```bash
    export VCPKG_ROOT="$HOME/vcpkg"
    ```

- Configure the game for building with this command: 

    ```bash
    cmake --preset ninja-x86_mingw_static_vcpkg-release
    ```

    `cmake` will begin configuriation and grab *all* the required dependecies needed to compile the game via `vcpkg`.

    <img src="./cmake-configure.png">

    Since this is your first time running the configuration, it may take some time, so be patient.\
    Future configurations will be faster.

- If configuration completes successfully, you _should_ see messages in the terminal like this:

    ```bash
    -- Configuring done (30.9s)
    -- Generating done (0.3s)
    -- Build files have been written to: C:/Users/SURANI-PC/RingRacersRepo/build/ninja-x86_mingw_static_vcpkg-release
    ```

    ---
    <sup>:star:</sup> <small>To avoid having to do this **all** the time, you can set `VCPKG_ROOT` as an environment variable _permanently_:
    ##### 1. Open the shell configuration file `(~/.bashrc)` using `nano` with the following command:

    ```bash
    nano ~/.bahsrc
    ```
    <img src="./nano-edit-bashrc.png">

    ##### 2. Add the following line to the file:

    ```bash
    export VCPKG_ROOT="$HOME/vcpkg"
    ```

    ##### 3. Press `Ctrl + O` to save the file, and press `Enter` to confirm the file name.

    ##### 4. Exit `nano` by pressing `Ctrl + X`.

    ---
    <small>If you're unsure, refer to the following video:</small>
    > <details><summary>Video reference</summary><video src="./vcpkg-bashrc.mp4" controls></details>
    </small>

### 6. Compiling the game

- To begin compiling, run the following command in the terminal:

    ```bash
    cmake --build --preset ninja-x86_mingw_static_vcpkg-release
    ```

    `cmake` will finally begin the build process, compiling the source files required to build the game's executable.\
    Depending on your computer's hardware, this can either be quick or take some time.


    ---
    <small>If you're unsure, refer to the following video:</small>
    > <details><summary>Compiling the game with cmake</summary><video src="./cmake-game-compile.mp4" controls></details>


- If the game has succesfully compiled, you should see a message in the terminal similar to this:

    ```bash
    [475/475] Linking CXX executable bin\ringracers_v2.3.exe
    ```

    This line confirms that the build process has completed and the executable has been succesfully created.

- The executable can be found in the `build` directory: 

    ```
    build/ninja-x86_mingw_static_vcpkg-release/bin
    ```

    <img src="./compiled-executable-ls.png">

    This path is relative to the `RingRacers` directory. The terminal opens in your home directory by default (`C:/Users/<your username>`).\
    Therefore, the full path to your compiled game would be:
    ```
    C:/Users/<your username/RingRacersRepo/build/ninja-x86_mingw_static_vcpkg-release/bin/ringracers_v2.3.exe
    ```

    Based on the example provided in this this guide, the full path to the compiled game would be:

    ```
    C:/Users/SURANI-PC/RingRacersRepo/build/ninja-x86_mingw_static_vcpkg-release/bin/ringracers_v2.3.exe
    ```

    <img src="./executable-path-windows.png">

- To run the executable, you need to copy it into the folder where you've already installed Dr. Robotnik's Ring Racers.
</details>
