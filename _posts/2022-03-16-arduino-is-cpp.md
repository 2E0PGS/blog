---
layout: post
title: Arduino is C++
date: 2022-03-16 21:17:19
author: Peter Stevenson
summary: What really is the Arduino language?
categories: programming
thumbnail:
tags:
 - Arduino
---

## Is Arduino C++?

A friend asked the following questions which provoked some thought:

* \> Is Arduino its own language?
* \> Is Arduino based on Processing, Java, C++ or C?
* \> Are Arduino sketches/programs classed as a firmware?

## TLDR

More of a domain language.

avr-gcc which includes avr-g++

So Ardunio (`arduino-1.8.8`) is C++ 11 (`-std=gnu++11`) but without any standard libraries so it can have optimised versions that consume less memory and are safer from leaks.

Yes an Arduino program/sketch would qualify as a firmware.

##  C++ IDE

* <https://docs.platformio.org/en/latest/integration/ide/atom.html>
* <https://platformio.org/install/ide?install=vscode>
* <https://marketplace.visualstudio.com/items?itemName=platformio.platformio-ide>

## Non C++ IDE

* Arduino IDE 1.8.8
* <https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino>
* Arduino IDE version 2.0.0-beta.3

## arduino-is-cpp.ino

```cpp
void setup() {
  Serial.begin(9600);
  while(!Serial){}
  Serial.println(__cplusplus);
}

void loop() {}
```

### Output

```
201103
```

## Verbose compiler output 

```
/home/peter/downloads/arduino-1.8.8/arduino-builder -dump-prefs -logger=machine -hardware /home/peter/downloads/arduino-1.8.8/hardware -tools /home/peter/downloads/arduino-1.8.8/tools-builder -tools /home/peter/downloads/arduino-1.8.8/hardware/tools/avr -built-in-libraries /home/peter/downloads/arduino-1.8.8/libraries -libraries /home/peter/Arduino/libraries -fqbn=arduino:avr:uno -vid-pid=0X2341_0X0043 -ide-version=10808 -build-path /tmp/arduino_build_342783 -warnings=none -build-cache /tmp/arduino_cache_309119 -prefs=build.warn_data_percentage=75 -prefs=runtime.tools.avr-gcc.path=/home/peter/downloads/arduino-1.8.8/hardware/tools/avr -prefs=runtime.tools.avr-gcc-5.4.0-atmel3.6.1-arduino2.path=/home/peter/downloads/arduino-1.8.8/hardware/tools/avr -prefs=runtime.tools.arduinoOTA.path=/home/peter/downloads/arduino-1.8.8/hardware/tools/avr -prefs=runtime.tools.arduinoOTA-1.2.1.path=/home/peter/downloads/arduino-1.8.8/hardware/tools/avr -prefs=runtime.tools.avrdude.path=/home/peter/downloads/arduino-1.8.8/hardware/tools/avr -prefs=runtime.tools.avrdude-6.3.0-arduino14.path=/home/peter/downloads/arduino-1.8.8/hardware/tools/avr -verbose /home/peter/downloads/sketch_may15a/sketch_may15a.ino
/home/peter/downloads/arduino-1.8.8/arduino-builder -compile -logger=machine -hardware /home/peter/downloads/arduino-1.8.8/hardware -tools /home/peter/downloads/arduino-1.8.8/tools-builder -tools /home/peter/downloads/arduino-1.8.8/hardware/tools/avr -built-in-libraries /home/peter/downloads/arduino-1.8.8/libraries -libraries /home/peter/Arduino/libraries -fqbn=arduino:avr:uno -vid-pid=0X2341_0X0043 -ide-version=10808 -build-path /tmp/arduino_build_342783 -warnings=none -build-cache /tmp/arduino_cache_309119 -prefs=build.warn_data_percentage=75 -prefs=runtime.tools.avr-gcc.path=/home/peter/downloads/arduino-1.8.8/hardware/tools/avr -prefs=runtime.tools.avr-gcc-5.4.0-atmel3.6.1-arduino2.path=/home/peter/downloads/arduino-1.8.8/hardware/tools/avr -prefs=runtime.tools.arduinoOTA.path=/home/peter/downloads/arduino-1.8.8/hardware/tools/avr -prefs=runtime.tools.arduinoOTA-1.2.1.path=/home/peter/downloads/arduino-1.8.8/hardware/tools/avr -prefs=runtime.tools.avrdude.path=/home/peter/downloads/arduino-1.8.8/hardware/tools/avr -prefs=runtime.tools.avrdude-6.3.0-arduino14.path=/home/peter/downloads/arduino-1.8.8/hardware/tools/avr -verbose /home/peter/downloads/sketch_may15a/sketch_may15a.ino
Using board 'uno' from platform in folder: /home/peter/downloads/arduino-1.8.8/hardware/arduino/avr
Using core 'arduino' from platform in folder: /home/peter/downloads/arduino-1.8.8/hardware/arduino/avr
Detecting libraries used...
/home/peter/downloads/arduino-1.8.8/hardware/tools/avr/bin/avr-g++ -c -g -Os -w -std=gnu++11 -fpermissive -fno-exceptions -ffunction-sections -fdata-sections -fno-threadsafe-statics -Wno-error=narrowing -flto -w -x c++ -E -CC -mmcu=atmega328p -DF_CPU=16000000L -DARDUINO=10808 -DARDUINO_AVR_UNO -DARDUINO_ARCH_AVR -I/home/peter/downloads/arduino-1.8.8/hardware/arduino/avr/cores/arduino -I/home/peter/downloads/arduino-1.8.8/hardware/arduino/avr/variants/standard /tmp/arduino_build_342783/sketch/sketch_may15a.ino.cpp -o /dev/null
Generating function prototypes...
/home/peter/downloads/arduino-1.8.8/hardware/tools/avr/bin/avr-g++ -c -g -Os -w -std=gnu++11 -fpermissive -fno-exceptions -ffunction-sections -fdata-sections -fno-threadsafe-statics -Wno-error=narrowing -flto -w -x c++ -E -CC -mmcu=atmega328p -DF_CPU=16000000L -DARDUINO=10808 -DARDUINO_AVR_UNO -DARDUINO_ARCH_AVR -I/home/peter/downloads/arduino-1.8.8/hardware/arduino/avr/cores/arduino -I/home/peter/downloads/arduino-1.8.8/hardware/arduino/avr/variants/standard /tmp/arduino_build_342783/sketch/sketch_may15a.ino.cpp -o /tmp/arduino_build_342783/preproc/ctags_target_for_gcc_minus_e.cpp
/home/peter/downloads/arduino-1.8.8/tools-builder/ctags/5.8-arduino11/ctags -u --language-force=c++ -f - --c++-kinds=svpf --fields=KSTtzns --line-directives /tmp/arduino_build_342783/preproc/ctags_target_for_gcc_minus_e.cpp
Compiling sketch...
/home/peter/downloads/arduino-1.8.8/hardware/tools/avr/bin/avr-g++ -c -g -Os -w -std=gnu++11 -fpermissive -fno-exceptions -ffunction-sections -fdata-sections -fno-threadsafe-statics -Wno-error=narrowing -MMD -flto -mmcu=atmega328p -DF_CPU=16000000L -DARDUINO=10808 -DARDUINO_AVR_UNO -DARDUINO_ARCH_AVR -I/home/peter/downloads/arduino-1.8.8/hardware/arduino/avr/cores/arduino -I/home/peter/downloads/arduino-1.8.8/hardware/arduino/avr/variants/standard /tmp/arduino_build_342783/sketch/sketch_may15a.ino.cpp -o /tmp/arduino_build_342783/sketch/sketch_may15a.ino.cpp.o
Compiling libraries...
Compiling core...
Using precompiled core: /tmp/arduino_cache_309119/core/core_arduino_avr_uno_dbe74caced8262d937c72ad4123d41cb.a
Linking everything together...
/home/peter/downloads/arduino-1.8.8/hardware/tools/avr/bin/avr-gcc -w -Os -g -flto -fuse-linker-plugin -Wl,--gc-sections -mmcu=atmega328p -o /tmp/arduino_build_342783/sketch_may15a.ino.elf /tmp/arduino_build_342783/sketch/sketch_may15a.ino.cpp.o /tmp/arduino_build_342783/../arduino_cache_309119/core/core_arduino_avr_uno_dbe74caced8262d937c72ad4123d41cb.a -L/tmp/arduino_build_342783 -lm
/home/peter/downloads/arduino-1.8.8/hardware/tools/avr/bin/avr-objcopy -O ihex -j .eeprom --set-section-flags=.eeprom=alloc,load --no-change-warnings --change-section-lma .eeprom=0 /tmp/arduino_build_342783/sketch_may15a.ino.elf /tmp/arduino_build_342783/sketch_may15a.ino.eep
/home/peter/downloads/arduino-1.8.8/hardware/tools/avr/bin/avr-objcopy -O ihex -R .eeprom /tmp/arduino_build_342783/sketch_may15a.ino.elf /tmp/arduino_build_342783/sketch_may15a.ino.hex
/home/peter/downloads/arduino-1.8.8/hardware/tools/avr/bin/avr-size -A /tmp/arduino_build_342783/sketch_may15a.ino.elf
Sketch uses 1634 bytes (5%) of program storage space. Maximum is 32256 bytes.
Global variables use 188 bytes (9%) of dynamic memory, leaving 1860 bytes for local variables. Maximum is 2048 bytes.
```

## Research links

* [Arduino Forum - Changing/Finding Out The C++ Compiler Version](https://forum.arduino.cc/t/changing-finding-out-the-c-compiler-version/604413/2)
* [Wikipedia - Wiring (development platform)](https://en.wikipedia.org/wiki/Wiring_(development_platform))
	* More of a domain language.
	* \> The Wiring IDE includes a C/C++ library called "Wiring", which makes common input/output operations much easier. Wiring programs are written in C++.
* [Wikipedia - Processing (programming language)](https://en.wikipedia.org/wiki/Processing_(programming_language))
	* \> Processing has spawned another project, Wiring, which uses the Processing IDE with a collection of libraries written in the C++ language
	* This is where the confusion comes in around Java. However this is only relevant to the Processing language which isn't what Arduino uses.
		* \> Written in	Java, GLSL, JavaScript
		* \> When programming in Processing, all additional classes defined will be treated as inner classes when the code is translated into pure Java before compiling.
* [Wikipedia - Arduino](https://en.wikipedia.org/wiki/Arduino)
	* \> The Arduino IDE supports the languages C and C++ using special rules of code structuring. The Arduino IDE supplies a software library from the Wiring project, which provides many common input and output procedures.
* [Stack Overflow - How to program an Arduino with C++](https://stackoverflow.com/questions/18523577/how-to-program-an-arduino-with-c)
* [GitHub - ladislas/Bare-Arduino-Project](https://github.com/ladislas/Bare-Arduino-Project)
* [Arduino Forum - What is the difference between std::string and String](https://forum.arduino.cc/t/what-is-the-difference-between-std-string-and-string/641910)
* [Stack Overflow -Strings in C++ class file for Arduino not compiling](https://stackoverflow.com/questions/10612385/strings-in-c-class-file-for-arduino-not-compiling)
* [Arduino Forum - Error \#include \<string\> while compileing](https://forum.arduino.cc/t/error-include-string-while-compileing/411001)

