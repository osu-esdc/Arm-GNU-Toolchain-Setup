# STM32-Setup_Guide
This repo contains setup guides for Windows and Linux users who want to program and debug STM32s using Vi/Vim, OpenOCD, STLink, and the Arm GNU Toolchain.

Base-level links you will need:  
[GNU Arm Toolchain](https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads)  -> Download the relevant .zip folder (for Windows) OR .tar.xz folder (for Linux)  
[OpenOCD](https://github.com/xpack-dev-tools/openocd-xpack/releases) -> Download the relevant darwin .tar.gz file (for Windows) OR the linux .tar.gz file (for Linux)  
[ST-Link for Windows 7, 8, or 10](https://www.st.com/en/development-tools/stsw-link009.html#get-software) -> Download if you have Windows

Some syntax notes:  
- angle brackets hold a name that is unique/different for various users. Fill these out with the relevant file name.  
- These instructions assume that downloaded files from the web go to ~/Downloads; if they don't, then use the pathname of the folder the downloaded files go to in place of ~/Downloads.
  
## For Linux (ubuntu 22.04 LTS):  

#### Install gcc  
```
sudo apt-get install gcc make binutils
```

#### Install dependencies   
```
sudo apt install libncurses5
```

#### Install GNU-Arm Toolchain (first download the needed .tar.xz folder)  
```
cd ~/Downloads
tar -xf <name of downloaded .tar.xz folder; should start with arm-gnu>
cd /opt
mkdir arm-gnu
cd ~/Downloads
sudo mv <arm-gnu folder name> /opt/arm-gnu
export PATH="/opt/arm-gnu/<arm-gnu folder name>/bin/:$PATH"
echo $PATH
```
-> should see arm-gnu/<arm-gnu folder name>bin/ somewhere in the echoed output.  

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
sudo mv <xpack folder name> /opt/openOCD
export PATH="/opt/openOCD/<xpack folder name>/bin/:$PATH"
echo $PATH
```
-> should see openOCD/<xpack folder name>/bin/ somewhere in the echoed output.  

#### Install STlink
```
sudo apt install stlink-tools
```

#### Adding some stuff to allow ST-link to be registered as a valid device
###### For ST-Link V2-1 connections (for other connections, see [this link](https://github.com/arduino/OpenOCD/blob/master/contrib/60-openocd.rules) to find the correct config data to put into 70-local.rules):  
```
sudo vi /etc/udev/rules.d/70-local.rules
```
Add the following line and then exit using `:wq`:
```
ATTRS{idVendor}=="0483", ATTRS{idProduct}=="374b", MODE="660", GROUP="plugdev", TAG+="uaccess"
```
```
sudo udevadm control --reload-rules
```

#### Add this stuff to PATH (permanently), IMPORTANT:
```
sudo vi /etc/environment
```
Insert the following at the end of the PATH variable:  
`/opt/arm-gnu/<arm-gnu folder name>/bin:/opt/openOCD/<xpack folder name>/bin:/opt/stlink/src`

... So that your PATH variable looks something like this:
PATH="/usr/local/sbin:/usr/local/bin:/opt/arm-gnu/<arm-gnu folder name>/bin:/opt/openOCD/<xpack folder name>/bin:/opt/stlink/src"








