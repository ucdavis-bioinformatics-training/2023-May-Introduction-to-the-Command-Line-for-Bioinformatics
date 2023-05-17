Setting up Windows 10/11 for Command-Line
---------------------------------------

In order to get a command line terminal for Windows 10, you will need to enable the WSL (Windows Subsystem for Linux) and install Ubuntu. Follow [these instructions](https://linuxhint.com/install_ubuntu_windows_10_wsl/) for installation.

Once you have installed the Ubuntu terminal, open one and copy and paste these commands:

	sudo apt install unzip
	sudo apt install make
	sudo apt install cmake


Setting up a Mac for Command-Line
-----------------------------------

A Mac is already set up for the command-line, but you will probably need to install "homebrew" and then use "brew" to install some other packages. So first [open a Terminal](https://www.wikihow.com/Open-a-Terminal-Window-in-Mac) and then go to the [homebrew page](https://brew.sh/) and follow the instructions. You basically need to copy and paste this line:

    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

into the Terminal and press Enter. The installer will explain what it is doing and it may need to install some other packages, such as Xcode. It will probably also ask for the admin password. Once the installer is done, you can use the **brew** command to install different packages. Install **gsed** and **wget** by typing these commands into the terminal:

	brew install wget
	brew install gnu-sed
    brew install coreutils
    brew install make
    brew install cmake
