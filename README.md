
[![Arduino CI](https://github.com/RobTillaart/ADT7470/workflows/Arduino%20CI/badge.svg)](https://github.com/marketplace/actions/arduino_ci)
[![Arduino-lint](https://github.com/RobTillaart/ADT7470/actions/workflows/arduino-lint.yml/badge.svg)](https://github.com/RobTillaart/ADT7470/actions/workflows/arduino-lint.yml)
[![JSON check](https://github.com/RobTillaart/ADT7470/actions/workflows/jsoncheck.yml/badge.svg)](https://github.com/RobTillaart/ADT7470/actions/workflows/jsoncheck.yml)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](https://github.com/RobTillaart/ADT7470/blob/master/LICENSE)
[![GitHub release](https://img.shields.io/github/release/RobTillaart/ADT7470.svg?maxAge=3600)](https://github.com/RobTillaart/ADT7470/releases)


# ADT7470 Library

Arduino library for I2C ADT7470 Fan Monitoring


## Description

The ADT7470 Fan Monitoring library offers an I2C device that can
monitor and control up to four fans. Further this module can daisy 
chain up to 10 (specific TMP05/06) temperature sensors.

Please read datasheet carefully before working with the module.

**Experimental**  
This library was build in 2015 from datasheet (PDF) on request and
is never tested by me. So it is experimental at best and if you have the 
hardware and are able to try this library I would really appreciate it
as it is a quite unique module.

That said the library is supporting setting the fan speed and measure 
the RPM, so it should be usable e.g. for a climate controlled room or
cabinet.

**Warning**
Do not forget to put a diode over the Fan to prevent damage due to
inductive pulse when switched off.


## ADT7470 Address Select Mode

(from datasheet)

| Pin 11 | (ADDR) State | Address |
|:------:|:------------:|:-------:|
| High (10 kΩ to VCC)   | 010 1111 (0x5E left-justified or 0x2F right-justified) | 
| Low (10 kΩ to GND)    | 010 1100 (0x58 left-justified or 0x2C right-justified) |
| Floating (no pull-up) | 010 1110 (0x5C left-justified or 0x2E right-justified) |


## Interface

The interface consists of:

- **ADT7470()** constructor
- **void begin()** initialize the I2C bus
- **bool isConnected()** check if the module is connected to the I2C bus
- **uint8_t getRevision()** version of the firmware
- **uint8_t getDeviceID()** should return 0x70
- **uint8_t getCompanyID()** should return 0x41
- **void startMonitoring()** 
- **void stopMonitoring()**
- **void powerDown()** energy save mode
- **void powerUp()** active mode
- **int8_t getTemperature(uint8_t idx)** idx = 0..9; if connected it returns the temperature 
of sensor idx. Temperature sensors are daisy changed.
- **int8_t getMaxTemperature()** get max temperature of connected temperature sensors.
- **bool setTemperatureLimit(uint8_t idx, int8_t low, int8_t high)** for ALARM function
- **int8_t getTemperatureLowLimit(uint8_t idx)**
- **int8_t getTemperatureHighLimit(uint8_t idx)**
- **bool setPWM(uint8_t idx, uint8_t val)** set the speed of the fan at idx
- **uint8_t getPWM(uint8_t idx)** read back the speed set. 
- **bool setFanLowFreq(val = 0)** 
- **bool setFanHighFreq(val = 0)** 
- **void setInvertPWM(uint8_t idx)**
- **uint8_t getInvertPWM(uint8_t idx)**
- **bool setPulsesPerRevolution(uint8_t idx, uint8_t val)** val should be 1..4 as a fan gives 1..4 pulses per revolution. 
This value is needed to calculate a correct tach and RPM.
- **uint8_t getPulsesPerRevolution(uint8_t idx)** read back PulsePerRevolution. returns 1..4.
- **void setFastTach()** Tach register is updated 4x per second.
- **void setSlowTach()** Tach register is updated 1x per second. 
- **uint16_t getTach(uint8_t idx)** get the raw pulses.
- **uint32_t getRPM(uint8_t idx)** get Revolutions Per Minute, based upon **getTach()**
- **bool setTachLimits(uint8_t idx, uint16_t low, uint16_t high)** 
- **uint16_t getTachLowLimits(uint8_t idx)** 
- **uint16_t getTachHighLimits(uint8_t idx)** 
- **uint16_t getTemperatureIRQstatus()**
- **void setTemperatureIRQMask(uint8_t idx)**
- **void clrTemperatureIRQMask(uint8_t idx)**
- **uint8_t getTemperatureIRQMask(uint8_t idx)**
- **uint8_t getFanIRQstatus()**
- **void setFanIRQMask(uint8_t idx)**
- **void clrFanIRQMask(uint8_t idx)**
- **uint8_t getFanIRQMask(uint8_t idx)**

The descriptions are short and need to be extended. 


## Todo / investigate / not implemented yet

- get the hardware to test 
- change pins from PWM to digital IO
- temperature sensors    (functions are prepared)
- How to connect temp sensors  (daisy chained)  
https://ez.analog.com/temperature_sensors/f/discussions/77540/adt7470-and-tmp05-daisy-chain-temeparure-sensing
- FULLSPEED pin, must it be in the library?  
software version ==> fullspeed(idx)
- auto mode
- improve documentation, readme.md file.
- ...


## Operation

See examples

