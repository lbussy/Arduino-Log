ArduinoLog - C++ Log library for Arduino devices
====================
[![Build Status](https://travis-ci.org/thijse/Arduino-Log.svg?branch=master)](https://travis-ci.org/thijse/Arduino-Log)
[![License](https://img.shields.io/badge/license-MIT%20License-blue.svg)](http://doge.mit-license.org)

:exclamation: This is forked from [thijse](https://github.com/thijse) / [Arduino-Log](https://github.com/thijse/Arduino-Log) (which in turn was forked from [mrRobot62](https://github.com/mrRobot62) / [Arduino-logging-library](https://github.com/mrRobot62/Arduino-logging-library)).  I did so because I rely upon it in my projects, and let's face it - sometimes things go away.

*An minimalistic Logging framework for Arduino-compatible embedded systems.*

ArduinoLog is a minimalistic framework to help the programmer output log statements to an output of choice, fashioned after extensive logging libraries such as log4cpp ,log4j, and log4net. In case of problems with an application, it is helpful to enable logging so that you may locate the problem. ArduinoLog is designed so that log statements can remain in the code with minimal performance cost. In order to facilitate this, you may adjust the loglevel, and (once you thoroughly test your code,) all logging code can be compiled out. 

## Features

* Different log levels (Error, Info, Warn, Debug, Verbose )
* Supports multiple variables
* Supports formatted strings 
* Supports formatted strings from flash memory
* Fixed memory allocation (zero malloc)
* MIT License

## Tested for 

* All Arduino boards (Uno, Due, Mini, Micro, Yun, and others)
* ESP8266
* ESP32

## Downloading

[thijse](https://github.com/thijse) published his version of this library to the Arduino & PlatformIO package managers, but you can also download it from GitHub. 

- By directly loading fetching the Archive from GitHub: 
 1. Go to [https://github.com/thijse/Arduino-Log](https://github.com/thijse/Arduino-Log)
 2. Click the DOWNLOAD ZIP button in the panel on the
 3. Rename the uncompressed folder **Arduino-Log-master** to **Arduino-Log**.
 4. You may need to create the libraries subfolder if its your first library.  
 5. Place the **Arduino-Log** library folder in your **<arduinosketchfolder>/libraries/** folder. 
 5. Restart the IDE.
 6. For more information, [read this extended manual](http://thijs.elenbaas.net/2012/07/installing-an-arduino-library/)


## Quickstart

```c++
    Serial.begin(9600);
    
    // Initialize with log level and log output. 
    Log.begin   (LOG_LEVEL_VERBOSE, &Serial);
    
    // Start logging text and formatted values
    Log.errorln (  "Log as Error   with binary values             : %b, %B"    , 23  , 345808);
    Log.warning (F("Log as Warning with integer values from Flash : %d, %d"CR) , 34  , 799870);
```

[See examples/Log/Log.ino](examples/Log/Log.ino)

## Usage

### Initialisation

The logging library needs to be initialized with the log level of messages to show and the log output. The latter is often the Serial interface.
Optionally, you can indicate whether to show the log type (error, debug, others) for each line.

```
begin(int level, Print* logOutput, bool showLevel)
begin(int level, Print* logOutput)
```

The log levels available are

```
* 0 - LOG_LEVEL_SILENT     no output 
* 1 - LOG_LEVEL_FATAL      fatal errors 
* 2 - LOG_LEVEL_ERROR      all errors  
* 3 - LOG_LEVEL_WARNING    errors, and warnings 
* 4 - LOG_LEVEL_NOTICE     errors, warnings and notices 
* 5 - LOG_LEVEL_TRACE      errors, warnings, notices & traces 
* 6 - LOG_LEVEL_VERBOSE    all 
```

example

```
    Log.begin(LOG_LEVEL_ERROR, &Serial, true);
```

if you want to delete all logging code, uncomment `#define DISABLE_LOGGING` in `ArduinoLog.h`, doing so may significantly reduce your sketch/library size.

### Log events

The library allows you to log on different levels by the following functions:

```c++
void fatal   (const char *format, va_list logVariables); 
void error   (const char *format, va_list logVariables);
void warning (const char *format, va_list logVariables);
void notice  (const char *format, va_list logVariables);
void trace   (const char *format, va_list logVariables);
void verbose (const char *format, va_list logVariables);
```

You may use the following format strings to format the log variables:

```
* %s    display as a string (char*)
* %S    display as astring from flash memory (__FlashStringHelper* or char[] PROGMEM)
* %c    display as a single character
* %d    display as an integer value
* %l    display as a long value
* %x    display as a hexadecimal value
* %X    display as a hexadecimal value prefixed by `0x`
* %b    display as a binary number
* %B    display as a binary number, prefixed by `0b'
* %t    display as a boolean value "t" or "f"
* %T    display as a boolean value "true" or "false"
* %D,%F display as a double value
```

You can add new lines using the `CR` keyword or by using the *ln version of each of the log functions.

### Storing messages in Flash memory

Flash strings log variables can be stored and reused at several places to reduce the final hex size.

```c++
    const __FlashStringHelper * logAs = F("Log as");
    Log.fatal   (F("%S Fatal   with string value from Flash   : %s" CR    ) , logAs, "value"     );
    Log.error   (  "%S Error   with binary values             : %b, %B" CR  , logAs, 23  , 345808);
```

If you want to declare that string globally (outside of a function), you must use the PROGMEM macro instead.

```c++
const char LOG_AS[] PROGMEM = "Log as ";

void logError() {
    Log.error   (  "%S Error   with binary values             : %b, %B" CR  , PSTRPTR(LOG_AS), 23  , 345808);
}
```

### Examples

```c++
    Log.fatal     (F("Log as Fatal   with string value from Flash   : %s" CR    ) , "value"     );
    Log.errorln   (  "Log as Error   with binary values             : %b, %B"    , 23  , 345808);
    Log.warning   (F("Log as Warning with integer values from Flash : %d, %d" CR) , 34  , 799870);
    Log.notice    (  "Log as Notice  with hexadecimal values        : %x, %X" CR  , 21  , 348972);
    Log.trace     (  "Log as Trace   with Flash string              : %S" CR    ) , F("value")  );
    Log.verboseln (F("Log as Verbose with bool value from Flash     : %t, %T"  ) , true, false );
```

### Disable library

(Once you have thoroughly tested your code) you may compile out all logging code. Do this by uncommenting:
```c++
#define DISABLE_LOGGING 
```
in `Logging.h`. Doing so may significantly reduce your project size.

## Credit

Based on library by 
* [Bernd Klein](https://github.com/mrRobot62)  

Bugfixes & features by
* [rahuldeo2047](https://github.com/rahuldeo2047)
* [NOX73](https://github.com/NOX73)
* [Dave Hylands](https://github.com/dhylands)
* [Jos Hanon](https://github.com/Josha)
* [Bertrand Lemasle](https://github.com/blemasle)
* [Mikael Falkvidd](https://github.com/mfalkvidd)

## On using and modifying libraries

- [http://www.arduino.cc/en/Main/Libraries](http://www.arduino.cc/en/Main/Libraries)
- [http://www.arduino.cc/en/Reference/Libraries](http://www.arduino.cc/en/Reference/Libraries) 

## Copyright

ArduinoLog is provided Copyright © 2017,2018, 2019 under MIT License.
