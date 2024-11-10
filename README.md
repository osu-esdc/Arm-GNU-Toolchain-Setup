# <ins> Arm GNU Toolchain Setup Guide </ins>
#### This repo contains setup guides for Windows and Linux users who want to program and debug STM32s using Vi and VSCode, OpenOCD, STLink, and the Arm GNU Toolchain.  
  
## <ins> Required Downloads: </ins>
Download the Arm GNU Toolchain: get one of the x86_64 Linux hosted cross toolchain if your WSL instance is running Ubuntu on an x86 cpu.  
Because we are compiling to a 32-bit STM32 mcu, get this: `arm-gnu-toolchain-13.3.rel1-x86_64-arm-none-eabi.tar.xz`; should be a .tar.xz file
NOTE: Get a different one if your computer is not running either x86 64-bit architecture or x64 architecture.
[Arm GNU Toolchain](https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads)  
  
Download OpenOCD: get a linux .tar.gz file that matches your system's architechture. 
[OpenOCD](https://github.com/xpack-dev-tools/openocd-xpack/releases)
  
  
## <ins> For WSL 2 (only works with Ubuntu 22.04): </ins>
  
### Setup WSL (for those who have not yet set up WSL):
In Windows Powershell:
```
wsl.exe --install Ubuntu-22.04
```
After installation, restart your computer, and type in `wsl` into the search bar. Open wsl. 

### Setup git stuff in WSL:
Set git username and password:
```
git config --global user.name “username”
git config --global user.email “email”
git config --list
```
-> output of last command should show you your inputted username and email!

Set up Git SSH:
```
cd ~/.ssh
```
-> if this fails, then run 
```
mkdir ~/.ssh/
```
Create a ssh key for git:
```
ssh-keygen
Enter file in which to save the key (/home/username/.ssh/id_rsa): id_ed25519
```
Copy the output from the following command:
```
cat id_ed25519
```
Go to git website -> click your git pfp in the upper right -> settings -> SSH and GPG Keys -> New SSH Key -> create a name for the SSH key and paste the previously copied output into the large text field.   

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

### Some syntax notes:  
- Angle brackets hold a name that is unique/different for various users. Fill these out with the relevant file name.  
- These instructions assume that downloaded files from the web go to the /c/users/username/Downloads folder path in your Windows OS. If they don't, then replace the path
  with the one that contains the previous downloads.
- You only need to use 'username' and not just username when you have spaces in the username. This username refers to the Windows OS username. 

#### Install gcc and vim  
```
sudo apt-get install gcc make binutils
sudo apt-get install update
sudo apt-get install upgrade
sudo apt install vim
```

#### Change the default text editor to vim  
```
sudo vim ~/.bashrc
```
Add the following lines to the end:  
```
export EDITOR='vim'
export VISUAL='vim'
```
Run:
```
sudo update-alternatives --config editor
Press <enter> to keep the current choice[*], or type selection number: 3
```
```
source ~/.bashrc
```
#### Make it so that sudo doesn't require password
```
sudo visudo
```
This will open the sudoers file in the default text editor. Look for the line that contains the following:
```
%sudo   ALL=(ALL:ALL) ALL
```
Below that line, add the following:
```
<wsl username>  ALL=(ALL) NOPASSWD:ALL
```
Save the changes and exit the text editor.
Verify that the sudo configuration is correct by running a command with sudo:
```
sudo ls
```
-> it should execute the command without asking for a password!

#### Install dependencies for gdb
```
sudo apt install libncurses5
sudo apt-get install libncursesw5
```

#### Install GNU-Arm Toolchain (first download the needed .tar.xz folder)  
notes:   
- may need to change the name of the arm-gnu file based on differing architechtures/OS    
```
sudo mkdir /opt/arm-gnu
sudo mv /mnt/c/users/'username'/Downloads/arm-gnu-toolchain-13.3.rel1-x86_64-arm-none-eabi.tar.xz /opt/arm-gnu
cd /opt/arm-gnu
sudo tar -xf arm-gnu-toolchain-13.3.rel1-x86_64-arm-none-eabi.tar.xz
sudo rm arm-gnu-toolchain-13.3.rel1-x86_64-arm-none-eabi.tar.xz
export PATH="/opt/arm-gnu/arm-gnu-toolchain-13.3.rel1-x86_64-arm-none-eabi/bin/:$PATH"
echo $PATH
```
-> should see `arm-gnu/arm-gnu-toolchain-13.3.rel1-x86_64-arm-none-eabi/bin/` somewhere in the echoed output.  

#### Test if GNU Arm Toolchain works:  
```
arm-none-eabi-gcc
```
-> if it outputs a fatal error and says "no input files", then you have a successful install!  

#### Install OpenOCD (first download the needed linux .tar.gz file)
notes: 
- may need to change the name of the xpack file based on differing architechtures/OS  
```
sudo mkdir /opt/openOCD
sudo mv /mnt/c/users/'username'/Downloads/xpack-openocd-0.12.0-4-linux-x64.tar.gz /opt/openOCD
cd /opt/openOCD
sudo tar -xzf xpack-openocd-0.12.0-4-linux-x64.tar.gz
sudo rm xpack-openocd-0.12.0-4-linux-x64.tar.gz
```

#### Install STlink
```
sudo apt install stlink-tools
```

#### Add USB rules
Copy USB rules from openOCD to udev/rules.d folder:
```
sudo cp /opt/openOCD/xpack-openocd-0.12.0-4/openocd/contrib/60-openocd.rules /etc/udev/rules.d
```
Implement the rule changes:
```
sudo udevadm control --reload-rules
```

#### Add the stuff we downloaded to PATH (permanently), IMPORTANT:  
```
sudo vim ~/.bashrc
```
At the end of the file, add the following lines: 
```
export PATH="/opt/arm-gnu/arm-gnu-toolchain-13.3.rel1-x86_64-arm-none-eabi/bin/:$PATH"
export PATH="/opt/openOCD/xpack-openocd-0.12.0-4/bin/:$PATH"
```
Run the following to reset path var:
```
source ~/.bashrc
```
Verify that PATH has been updated: 
```
echo $PATH
```
-> should see the following:
```
/opt/openOCD/xpack-openocd-0.12.0-4/bin/:/opt/arm-gnu/arm-gnu-toolchain-13.3.rel1-x86_64-arm-none-eabi/bin/
```
somewhere in the PATH variable. 

#### Getting USB support in WSL:  
Follow the directions in this [link](https://learn.microsoft.com/en-us/windows/wsl/connect-usb)    
Download the .msi file from [here](https://github.com/dorssel/usbipd-win/wiki/WSL-support)  
Run the .msi file installer  

Before this step: make sure that the microcontroller you want to use is currently plugged into your machine!!!  
Open up command prompt as administrator, then follow these [directions](https://github.com/dorssel/usbipd-win/wiki/WSL-support)  

#### Reattaching device every time you unplug-re plug in the device to WSL (Only do this if you have already done the previous steps):  
In command prompt:
```
usbipd list
```
-> find the shared busid device
```
usbipd attach --wsl --busid <busid>
```

### Get VSCode Integration for WSL:   
Requirements:   
- VSCode downloaded on your Windows OS  
- WSL downloaded and set up
  
Follow the instructions at this [link](https://code.visualstudio.com/docs/remote/wsl)
  
###### You can use the VSCode terminal for the client side of openOCD, and a WSL terminal for the server side of openOCD. 

### Allow .gdbinit functions to be run in gdb:  
```
vim ~/.gdbinit
```
Add the following line to the file, then save and exit:  
```
set auto-load safe-path /
```

#### You are now ready to flash code! Just follow the directions in the Nucleo-L476RG-LED Repositor to program the STM32 Ref board for the first time.   




## <ins> For Linux (tested for ubuntu 22.04 LTS): </ins>
### Some syntax notes:  
- The names of the xpack and arm-gnu files may be different depending on your system's architecture or OS.
- These instructions assume that downloaded files from the web go to ~/Downloads; if they don't, then use the pathname of the folder the downloaded files go to in place of ~/Downloads.

#### Install gcc and vim
```
sudo apt-get install gcc make binutils
sudo apt install vim
```

#### Install dependencies for gdb
```
sudo apt install libncurses5
sudo apt-get install libncursesw5
```

#### Install GNU-Arm Toolchain (first download the needed .tar.xz folder)  
```
cd ~/Downloads
tar -xf <name of downloaded .tar.xz folder; should start with arm-gnu>
cd /opt
mkdir arm-gnu
cd ~/Downloads
sudo mv <arm-gnu unpacked folder name> /opt/arm-gnu
export PATH="/opt/arm-gnu/<arm-gnu unpacked folder name>/bin/:$PATH"
echo $PATH
```
-> should see `arm-gnu/<arm-gnu unpacked folder name>/bin/` somewhere in the echoed output.  

#### Test if GNU Arm Toolchain works:  
```
arm-none-eabi-gcc
```
-> if it outputs a fatal error and says "no input files", then you have a successful install!  

#### Install OpenOCD (first download the needed linux .tar.gz file)   
```
cd ~/Downloads
tar -xzf <name of linux .tar.gz file; should start with xpack>
cd /opt
mkdir openOCD
cd ~/Downloads
sudo mv <xpack unpacked folder name> /opt/openOCD
export PATH="/opt/openOCD/<xpack unpacked folder name>/bin/:$PATH"
echo $PATH
```
-> should see `/opt/openOCD/<xpack unpacked folder name>/bin/` somewhere in the echoed output.  

#### Install STlink
```
sudo apt install stlink-tools
```

#### Add USB rules
Copy USB rules from openOCD to udev/rules.d folder:
```
sudo cp /opt/openOCD/xpack-openocd-0.12.0-4/openocd/contrib/60-openocd.rules /etc/udev/rules.d
```
Implement the rule changes:
```
sudo udevadm control --reload-rules
```

#### Add the stuff we downloaded to PATH (permanently), IMPORTANT:  
```
sudo vim ~/.bashrc
```
At the end of the file, add the following lines: 
```
export PATH="/opt/arm-gnu/arm-gnu/<arm-gnu unpacked folder name>/bin/:$PATH"
export PATH="/opt/openOCD/<xpack unpacked folder name>/bin/:$PATH"
```
Run the following to reset path var:
```
source ~/.bashrc
```
Verify that PATH has been updated: 
```
echo $PATH
```
-> should see the following:
```
/opt/openOCD/<xpack unpacked folder name>/bin/:/opt/arm-gnu/arm-gnu/<arm-gnu unpacked folder name>/bin/
```
somewhere in the PATH variable. 

### Allow .gdbinit functions to be run in gdb:  
```
vim ~/.gdbinit
```
Add the following line to the file, then save and exit:  
```
set auto-load safe-path /
```
#### VSCode stuff: Just open the git repo in VSCode. I recommend running the client side of OpenOCD in VSCode and the server side of OpenOCD in a seperate terminal.

#### You are now ready to flash code! Just follow the directions in the Nucleo-L476RG-LED Repositor to program the STM32 Ref board for the first time.   

















