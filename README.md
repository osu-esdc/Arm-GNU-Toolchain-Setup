# Arm GNU Toolchain and OpenOCD Setup Guide 
- Supports Windows and Linux Hosts
- Sets up development environment for programming STM32 (among other) microcontrollers
- OpenOCD setup - A method to debug and program microcontrollers
- Arm GNU toolchain setup - A toolchain including cross compilers, binary code analyzers, an assembler, and more

&nbsp;
## <ins> Required Downloads: </ins>
### Download the Arm GNU Toolchain: 
- If your computer has an Intel or AMD CPU (x86_64), download `arm-gnu-toolchain-15.2.rel1-x86_64-arm-none-eabi.tar.xz` from 
<ins> [Arm GNU Toolchain](https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads) </ins>

- If your computer has an ARM-based CPU, download `arm-gnu-toolchain-15.2.rel1-aarch64-arm-none-eabi.tar.xz` from 
<ins> [Arm GNU Toolchain](https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads) </ins>


### Download OpenOCD: 
- If your computer has an Intel or AMD CPU (x86_64), 
download `xpack-openocd-0.12.0-7-linux-x64.tar.gz` from 
<ins> [OpenOCD](https://github.com/xpack-dev-tools/openocd-xpack/releases) </ins>

- If your computer has an ARM-based CPU, download `xpack-openocd-0.12.0-7-linux-arm64.tar.gz`
<ins> [OpenOCD](https://github.com/xpack-dev-tools/openocd-xpack/releases) </ins>

<ins> NOTE</ins> that this guide's commands use the x86_64 names for these files. If you have an ARM-based CPU, you will
need to adjust the names of these files accordingly. 
  
&nbsp;
## <ins> Installation Guide
### <ins> NOTE:</ins>
- Directions that start with `Windows Hosts` should be ran by users using Windows as their host OS. 
- Directions that start with `Linux Hosts` should be ran by users using Linux as their host OS. 
- Directions that start with `All Hosts` either should be ran by all users. 
- Guide has been tested on Ubuntu 24.04 Linux Host and Windows 10 Host. 
- Commands should be ran in Linux terminal unless explicitly stated otherwise (i.e., run in Command Prompt)

&nbsp;
### `Windows Hosts` Setup WSL (if not already set up):
Open Command Prompt as administrator, then run:
```
wsl --install
```
```
wsl.exe --install Ubuntu-24.04
```
```
wsl --set-default Ubuntu-24.04
```
After installation, restart your computer, and type in `wsl` into the search bar. Open wsl. 

&nbsp;
### `All Hosts` Install tools 
```
sudo apt update && sudo apt upgrade
sudo apt install gcc make binutils stlink-tools libncurses-dev 
```

&nbsp;
### `All Hosts` Set up environmental variables 
<ins> NOTE:</ins> Make sure you know what path your toolchain/OpenOCD files were downloaded to. If they
are different than the examples in this section, replace them with the actual paths. 
- `Windows Hosts` (make sure to replace <WinUser> with your Windows host username) </ins>:
```
export DOWNLOAD_DIR=/mnt/c/users/<WinUser>/Downloads
```
-> If WinUser has a space in it `(i.e., Shirley Temple)`, then surround it with single quotation marks `(i.e., 'Shirley Temple')`
- `Linux Hosts`
```
export DOWNLOAD_DIR=~/Downloads
```

&nbsp;
### `All Hosts` Install Arm-GNU Toolchain and OpenOCD
```
sudo tar -xf $DOWNLOAD_DIR/arm-gnu-toolchain-15.2.rel1-x86_64-arm-none-eabi.tar.xz -C /opt
```
```
sudo tar -xzf $DOWNLOAD_DIR/xpack-openocd-0.12.0-7-linux-x64.tar.gz -C /opt
```

&nbsp;
### `All Hosts` Add USB rules
Copy USB rules from openOCD to udev/rules.d folder:
```
sudo cp /opt/xpack-openocd-0.12.0-7/openocd/contrib/60-openocd.rules /etc/udev/rules.d
```
Implement the rule changes:
```
sudo udevadm control --reload-rules
```

&nbsp;
### `All Hosts` Add tool executables to PATH
<ins> NOTE:</ins> if you are not using bash as your shell, then replace bashrc with your chosen shell. Ubuntu defaults to bash. 
```
echo -e '\nexport PATH="/opt/arm-gnu-toolchain-15.2.rel1-x86_64-arm-none-eabi/bin:/opt/xpack-openocd-0.12.0-7/bin/:$PATH"' >> ~/.bashrc
```
```
source ~/.bashrc
```

&nbsp;
### `Windows Hosts` Get USB support in WSL
- Download the .msi file from [here](https://github.com/dorssel/usbipd-win/releases)  
- Run the .msi file installer  
- Before the next step: make sure that the microcontroller you want to use is currently plugged into your machine  

&nbsp;
### `Windows Hosts` Set up USB device for WSL
Add the Virtual USB port driver into a configuration file that runs at WSL boot:
```
echo "vhci_hcd" | sudo tee /etc/modules-load.d/wsl-usb.conf
```
Open Command Prompt as administrator, then run this: 
```
usbipd list
```
-> This command outputs the BUSIDs and names for any USB device connected to your computer. Find the BUSID associated with the ST-Link Debug device, then use it in the next command: 
```
usbipd bind --busid <BUSID>
usbipd attach --wsl --busid <BUSID>
```
Restart WSL: 
```
wsl --shutdown
```
-> Open WSL again. 

&nbsp;
### `Windows Hosts` Quick commands to re-attach USB device to WSL after every disconnect
In Command Prompt:
```
usbipd list
```
-> find the shared busid device
```
usbipd attach --wsl --busid <BUSID>
```
In WSL terminal, run the following command to check if the USB device has been attached correctly:
```
lsusb
```
-> Should see the name of the attached USB device in the output.

&nbsp;
### `All Hosts` Testing OpenOCD and GNU ARM Toolchain
Run the following commands:
```
arm-none-eabi-gcc --version
openocd --version
```
-> If the installation has succeeded, then you should see a message after each command stating the name of the tool and the version (15.2.Rel1 for arm-none-eabi-gcc and 0.12.0+dev... for openocd)!  

&nbsp;
## Optional (but recommended) extra configurations and advices

### `Windows Hosts` Optional: Get VSCode Integration for WSL   
Requirements:   
- VSCode downloaded on your Windows OS  
- WSL downloaded and set up  

Follow the instructions [here](https://code.visualstudio.com/docs/remote/wsl)  
  
&nbsp;
### `All Hosts` Optional: Allow .gdbinit functions to be run in gdb
```
vim ~/.gdbinit
```
Add the following line to the file, then save and exit:  
```
set auto-load safe-path /
```

&nbsp;
### `All Hosts` Optional: Set up Git SSH to push changes to the remote GitHub repository
Set git username and password:
```
git config --global user.name "username"
git config --global user.email "email"
git config --list
```
-> output of last command should show you your inputted username and email!

&nbsp;
### `All Hosts` Optional: Set up SSH keys for GitHub
Make the .ssh directory if it does not exist yet and then cd into it
```
mkdir ~/.ssh
cd ~/.ssh
```

Create an ssh key for git:  
```
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519
```
Copy the output from the following command:
```
cat ~/.ssh/id_ed25519.pub
```
Go to git website -> click your git pfp in the upper right -> settings -> SSH and GPG Keys -> New SSH Key -> 
create a name for the SSH key and paste the previously copied output into the large text field.   

Run the following to update ssh agent to use this key:
```
eval `ssh-agent -s`
ssh-add
```
Verify that your ssh stuff works:
```
ssh -T git@github.com
```
-> should see `Hi git_username! You've successfully authenticated, but GitHub does not provide shell access.`!

&nbsp;
### `Windows Hosts` Tips: Quick commands to re-attach USB device to WSL after every disconnect
In Command Prompt:
```
usbipd list
```
-> find the shared busid device
```
usbipd attach --wsl --busid <BUSID>
```
In WSL terminal, run the following command to check if the USB device has been attached correctly:
```
lsusb
```
-> Should see the name of the attached USB device in the output.

## You are now ready to flash code! Just follow the directions in the Nucleo-F446RE-LED Repository to program the club's STM32 reference boards for the first time, available [here](https://github.com/osu-esdc/Nucleo-F446RE-LED). 