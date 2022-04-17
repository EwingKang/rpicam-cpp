# raspicam-cpp
A C++ wrapper for Raspberry PI camera library, including callbacks, streaming, and encoding.

# Dependencies
This library is intended to be used as a submodule of other projects. However, it still can be compile with some modification. You can include this repo as submodule with cmake `add_subdirectory()` function.  
* [Raspberry Pi VideoCore ](https://github.com/raspberrypi/userland) under `/opt/vc`. This directory usually comes with the RPI image. It can also be install with apt, according to the "VideoCore" section of [this tutorial](https://wiki.ubuntu.com/ARM/RaspberryPi#Videocore), or can be built from source with [this](https://www.funtoo.org/Raspberry_Pi_Userland_(VCGENCMD)) or [this](http://dev1galaxy.org/viewtopic.php?id=2967) tutorial.
* [spdlog v1.8.5](https://github.com/gabime/spdlog/tree/v1.8.5)
* [cxxopts 43ce03fdb](https://github.com/jarro2783/cxxopts/tree/43ce03fdbd850385b0461c8873771dc47be5c9b3) For executable argument parsing if you opt-in to build test executables.
