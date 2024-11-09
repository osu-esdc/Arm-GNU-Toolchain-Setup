# STM32-Setup_Guide
This repo contains setup guides for Windows and Linux users who want to use STM32s using VScode/Vi

Base-level links you will need:  
[GNU Arm Toolchain](https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads)  -> Download the relevant .zip folder (for Windows) and .tar.xz folder (for Linux)  
[OpenOCD](https://github.com/xpack-dev-tools/openocd-xpack/releases) -> Download the .zip folder (for Windows) and the .tar folder (for Linux)  
[ST-Link for Windows 7, 8, or 10](https://www.st.com/en/development-tools/stsw-link009.html#get-software) -> Download if you have Windows


For Linux (ubuntu 22.04 LTS):

Install gcc  
```
sudo apt-get install gcc make binutils
```

Install GNU-Arm Tooolchain -> first download the needed .tar.xz folder  
```
cd ~/Downloads
```
tar -xjf <name of downloaded .tar.xz folder>
cd ~
cd opt
mkdir gcc-arm
