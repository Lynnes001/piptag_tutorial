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
 
  > libcurl

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
![finish](screen.png)
