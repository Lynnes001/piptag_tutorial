# Tutorial for PIP-Tag

### Hardware Prerequisite:
 1. USB receiver plugged in.
 2. PIP tag installed with a battery.

### Programs

File Structure

```
├── README.md
├── pip_sense.v2
├── pip_sense_layer.v2.cpp
├── sample_data.hpp
├── sensor_aggregator_protocol.hpp
└── simple_sockets.hpp
```

- .hpp files are dependency libraries that are required while compiling/making.  
- pip_sense_layer.v2.cpp is the original C++ file which contains the program.
- pip_sense.v2 is the compiled file from pip_sense_layer.v2.cpp.

 **How to compile/make file.**

- dependencies:
  > g++ gnu compiler

  > essential (install: `sudo apt-get install build-essential`)
 
  > usblib ( install: `sudo apt-get install libusb-dev`)
 
  > sample_data.hpp
 
  > simple_sockets.hpp
 
  > sensor_aggregator_protocol.hpp
 
  > *libcurl

- compile:

  - Open terminal in the folder and run

    `$ g++ -g -std=gnu++0x -o pip_sense.v2 pip_sense_layer.v2.cpp -lusb`
    
    And we would get file 'pip_sense.v2' in the current folder.
    
    *`g++` is the command to call g++ compiler. `-g` requests that the compiler and linker generate and retain symbol information in the executable itself ([click here](https://stackoverflow.com/questions/5179202/gcc-g-what-will-happen) for details) which makes it easy to debug. `-std=gnu++0x` set the C++ standard to 0x (like 08). `-o pip_sense.v2` set output mode to output compiled file namd as 'pip_sense.v2 saving in the save folder'. `-lusb` links two libraries to compiler.

 **How to run and collect data in the terminal.**

- general: 
  
  `$ sudo stdbuf -o0 ./pip_sense.v2 l l | stdbuf -o0 grep TX:0$1 |tee $2`

- my: (in order to receive msg from Tag: 03377)
  
  `$ sudo stdbuf -o0 ./pip_sense.v2 l l | stdbuf -o0 grep TX:03377`
  
  *`sudo` adminstrator permission is required since it uses usb connection. `stdbuf -o0` set buffering as none. `./pip_sense.v2 l l`, the path of the file follows with two parameters means using localhost. `grep` use a regular expression to match 03377 or 0$1. `tee` redriect data stream to both the screen and the file.
   
Result:
![finish](images/screen.png)



# How to flash the PIP tag (optional)

### Hardware Prerequisite:
 1. TI MSP-EXP430G2 LaunchPad.
 2. mini USB cable.
 3. PIP tag.
 4. USB receiver plugged in. (for debugging)

### Programs
- Required IDE: Code Composer Studio (Checked avalible on Version: 6.1.1)

File Structure (important files listed only)
Provided code
```
PIP1_SendBeacon
├── ...
├── main.c
├── main.h
├── floatToBits.cpp
├── settings.h
├── ...
```

Steps for flashing

 1. open IDE
 ![CCS IDE](images/IDE.png)
 
 2. in setting.h line 56 
 
 ```
 #define TXER_ID 3378	//0xABBADABA is default ID for automatic flashing
 ```
 
 // more options for setting.h
 
 Change the `3378` to the ID of the PIP tag you want to flash, it is 3378 for my PIP tag.
 
 3. Connect the program LaunchPad to PC and PIP tag like this. Remember to uninstall the battery before flashing.
  ![CODE FLASH SETUP](images/setup.jpg)
 
 4. Simply click build, debug and result button in debug view in Code Composer step by step. 
 
 
### Troubleshooting
  
  1. (from original readme.md)Due to the use of GCC attributes for packing structs, you need to enable GCC compatibility mode. This is an option found in the Project Properties dialog window.

  Project > Properties > CCS Build > MSP430 Compiler > Advanced Options 
    > Language Options > Enable support for GCC extensions (--gcc)

You also need to exclude "floatToBits.cpp" from the build if it is causing
errors.  Be sure to select all builds you have configured in the dialog
window.

  Right-Click > Resource Configurations > Exclude from Build...
  
  2. The original program is created with compiler version 3.3.3 but is no longer downloadable, use version 4.4.5 as an alternative.


