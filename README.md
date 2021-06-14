# zephyr-shell

This demo has been prepared in order to test all available features for Renesas R-Car Gen3 boards.

The current version supports features that may only be available in Iot.bzh Renesas 2.6 BSP.

Knowing that it is not possible to access secondary UART on the Ebisu board, this demo is mainly provided for H3ULCB board.

## Build

Building for H3ULCB:

```bash
west build -b rcar_h3ulcb_cr7
```

## How to use

Once your zephyr-shell app is flashed on a H3ULCB board, here are the useful commands:

__Note__ : copy paste may brake zephyr-shell interpreter

### GPIO

__Physical specs:__

- GPIO6 Pin 12 -> H3 LED5

__Available commands:__

```bash
gpio set gpio6 12 1  #Turn LED5 on
gpio set gpio6 12 0  #Turn LED5 off
gpio get gpio6 12    #Get LED5 status
```

## I2C

__Physical specs:__

- Bus i2c2 & i2c4
- TCA9539 I/O expander on 0x74 and 0x75 (i2c2)
- Register 0x02 is output port 0 (default value = 0xFF)

__Commands:__

```bash
i2c read_byte i2c2 0x74 0x02         #Get "output 0" current value for 0x74 addressed TCA9539
i2c write_byte i2c2 0x74 0x02 0x00   #Set output 0 to 0x00 for 0x74 addressed TCA9539
i2c write_byte i2c2 0x74 0x02 0xff   #Set output 0 to 0xff for 0x74 addressed TCA9539
```

## CAN

__Physical specs:__

- Bus CAN0 -> CN17 connector on KingFisher
- Pinout : 1 CAN0H, 2 CAN0L, 3 GND

__Commands:__

```bash
canbus send can0 0x123 0xDE 0xAD 0xBE 0xEF          #Send DEADBEEF with ID=0x123 on can0
canbus send can0 -e 0x12345678 0xDE 0xAD 0xBE 0xEF  #Send DEADBEEF with extID=0x12345678 on can0
canbus attach can0 0x001                  #Listen for messages with ID=0x001 on can0 (Return Filter ID)
canbus attach can0 -e 0x12345678          #Listen for messages with ID=0x12345678 on can0 (Return Filter ID)
canbus detach can0 0                      #Stop listening for Filter 0
canbus config can0 250000                 #Set can0 bitrate at 250000 kbit/s
```
