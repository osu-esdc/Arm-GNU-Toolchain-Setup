# STM32-Setup_Guide
This repo contains setup guides for Windows and Linux users who want to use STM32s using VScode/Vi

Base-level links you will need:  
[GNU Arm Toolchain](https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads)  -> Download the relevant .zip folder (for Windows) OR .tar.xz folder (for Linux)  
[OpenOCD](https://github.com/xpack-dev-tools/openocd-xpack/releases) -> Download the relevant darwin .tar.gz file (for Windows) OR the linux .tar.gz file (for Linux)  
[ST-Link for Windows 7, 8, or 10](https://www.st.com/en/development-tools/stsw-link009.html#get-software) -> Download if you have Windows


### For Linux (ubuntu 22.04 LTS):  

Install gcc  
```
sudo apt-get install gcc make binutils
```

Install GNU-Arm Tooolchain -> first download the needed .tar.xz folder; if ~/Downloads is not where this folder is placed then replace Downloads with that folder  
```
cd ~/Downloads
tar -xjf <name of downloaded .tar.xz folder>
cd /
cd opt
mkdir gcc-arm
cd ~/Downloads
sudo mv <name of unpacked .tar.xz folder> /opt/gcc-arm
cd opt
sudo mv gcc-arm temp
sudo mv temp/<name of unpacked .tar.xz folder> gcc-arm
sudo rmdir temp
export PATH="/opt/gcc-arm/bin/:$PATH"
echo $PATH
```
-> should see the path name of your gcc-arm/bin folder in the echoed output.  

Test if GNU Arm Toolchain works:  
```
arm-none-eabi-gcc
```
-> if it outputs a fatal error and says "no input files", then you have a successful install!  



