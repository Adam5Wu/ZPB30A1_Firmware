# ZPB30A1 Firmware

This repository is there to build an open-source firmware for the ZPB30A1 electronic load. The original Firmware is protected and can therefore not been read and improved partially.

## Hardware

The ZPB30A1 uses a STM8S005K6 running at 12 MHz.

### Processor specifications

- 32 kByte Flash
- 2 kByte RAM
- 128 Byte EEPROM
- External interrupts in almost all pins
- Up to 6 analog inputs in LQFP32 package
- 2x 16 Bit, 1x 8 Bit timers
- UART, SPI, I<sup>2</sup>C

### Pinout

| Pin  | Pin Name | Direction | Function                 | Note
| ---: | -------- | --------- | ------------------------ | ---
|   1  | NRST     | IN PUP    | NRST                     | Connect to ST-Link
|   2  | PA1      | XIN       | Crystal                  | 12 MHz
|   3  | PA2      | XOUT      | Crystal                  | 12 MHz
|   4  | VSS      | PWR       | GND                      | Ground
|   5  | VCAP     | PAS       | VCAP                     | 1uF for internal 1.8V regulator
|   6  | VDD      | PWR       | Vcc                      | 5V
|   7  | VDDIO    | PWR       | Vcc                      | 5V
|   8  | PF4/AIN12| -/-       | NC                       | Ground
|   9  | VDDA     | PWR       | Vcc                      | 5V (via PCB inductor)
|  10  | VSSA     | PWR       | GND                      | Ground
|  11  | PB5/AIN5 | IN PUP    | Encoder 2                | Frontpanel
|  12  | PB4/AIN4 | IN PUP    | Encoder 1                | Frontpanel
|  13  | PB3/AIN3 | IN Analog | Input voltage            | `VIN / 3`
|  14  | PB2/AIN2 | IN Analog | Sense voltage            | `0.137565 V * V_SENSE + 0.0965910 V`
|  15  | PB1/AIN1 | IN Analog | Load voltage             | `0.121221 V * V_POWER + 0.0847433 V`
|  16  | PB0/AIN0 | IN Analog | Thermistor input         | <!--`t = 121.104 - 22.8585 * V_TEMP` (±1°C)-->
|  17  | PE5      | OUT       | Enable                   | Must be LOW to enable load regulation, otherwise MOSFET stays off (no load)
|  18  | PC1/T1C1 | OUT PWM   | I-Set                    | `I = 12.9027 * DUTY_PERCENT + 0.0130276` (800Hz)
|  19  | PC2/T1C2 | IN PUP    | Overload detect          | LOW when SET I is higher than the source can deliver
|  20  | PC3/T1C3 | IN PUP    | Encoder Button           | Frontpanel
|  21  | PC4/T1C4 | IN PUP    | Run Button               | Frontpanel
|  22  | PC5/SCK  | OUT       | Soft-I<sup>2</sup>C SCK  | Frontpanel
|  23  | PC6/MOSI | I/O       | Soft-I<sup>2</sup>C SDA1 | Frontpanel
|  24  | PC7/MISO | I/O       | Soft-I<sup>2</sup>C SDA2 | Frontpanel
|  25  | PD0/T3C2 | OUT PWM   | FAN                      | PWM speed control
|  26  | PD1/SWIM | PROG      | SWIM                     | Connect to ST-Link
|  27  | PD2/T3C1 | OUT PWM   | F                        | 50 kHz 50% duty
|  28  | PD3/T2C2 | IN PUP    | Voltage OK               | LOW when VIN > 9.6 V
|  29  | PD4/T2C1 | OUT PWM   | Buzzer                   | 2.364 kHz 50% duty
|  30  | PD5/U2TX | OUT       | Tx                       | 115200 baud 8n1
|  31  | PD6/U2RX | IN        | Rx                       | 115200 baud 8n1
|  32  | PD7/TLI  | IN        | L                        | Interrupt

## Software

It seems like they have activated the Read Out Protection, so when running a flash readout, it just dumps zeros. So there is no real software reverse engineering possible. Software needs to be build from scratch.

As there are just a few compilers out there and just one open source solution, we'll be using [Small Device C Compiler (SDCC)](http://sdcc.sourceforge.net/).

### Planned functions

Currently this repository does not contain that many information, as this will be my first project with STM8 controllers, so no software ready yet. But I have big dreams ;-)

- Different modes
 - CC (Constant Current, as default)
 - CW (Constant Power)
 - CR (Constant Resistance)
 - CV (Constant Voltage)
- Continous output of data via UART
- Logging of Ah, Wh, J (?) in every mode
- Adjusting shunt resistance (if replaced with e.g. 100 mΩ for an offset as low as 20 mA instead of 200 mA, not the resistance itself but the value in software)
- Toggle beeper, auto shutdown
- Nice menus even we got just that small seven segments