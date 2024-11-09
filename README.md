# STM32-Setup_Guide
This repo contains setup guides for Windows and Linux users who want to use STM32s using VScode/Vi

Base-level links you will need:  
[GNU Arm Toolchain](https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads)  -> Download the relevant .zip folder (for Windows) OR .tar.xz folder (for Linux)  
[OpenOCD](https://github.com/xpack-dev-tools/openocd-xpack/releases) -> Download the relevant darwin .tar.gz file (for Windows) OR the linux .tar.gz file (for Linux)  
[ST-Link for Windows 7, 8, or 10](https://www.st.com/en/development-tools/stsw-link009.html#get-software) -> Download if you have Windows


## For Linux (ubuntu 22.04 LTS):  

#### Install gcc  
```
sudo apt-get install gcc make binutils
```

#### Install dependencies
```
sudo apt install lubncurses5
```

#### Install GNU-Arm Tooolchain -> first download the needed .tar.xz folder; if ~/Downloads is not where this folder is placed then replace Downloads with that parent folder  
```
cd ~/Downloads
tar -xf <name of downloaded .tar.xz folder; should start with arm-gnu>
cd /opt
mkdir arm-gnu
cd ~/Downloads
sudo mv <arm-gnu .tar.xz folder> /opt/arm-gnu
export PATH="/opt/arm-gnu/<arm-gnu .tar.xz folder>/bin/:$PATH"
echo $PATH
```
-> should see arm-gnu/bin/ somewhere in the echoed output.  

#### Test if GNU Arm Toolchain works:  
```
arm-none-eabi-gcc
```
-> if it outputs a fatal error and says "no input files", then you have a successful install!  

#### Install OpenOCD -> first download the needed linux .tar.gz file; if ~/Downloads is not where this folder is placed then replace Downloads with that parent folder
```
cd ~/Downloads
tar -xzf <name of linux .tar.gz file; should start with xpack>
cd /opt
mkdir openOCD
cd ~/Downloads
sudo mv <xpack .tar.gz folder> /opt/openOCD
export PATH="/opt/openOCD/<xpack .tar.gz folder>/bin/:$PATH"
echo $PATH
```
-> should see openOCD/<xpack .tar.gz folder>/bin/ somewhere in the echoed output.  






