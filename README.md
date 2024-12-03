# 使用Arduino UNO烧录ATMETA16U2

修复ATMEGA16U2串口通信固件



1，将烧录器和目标CPU的ISP线连好,

| ArduinoISP（UNO主板） | 目标板（ATMEGA16U2） |
| --------------------- | -------------------- |
| 10                    | RESET                |
| 11                    | MOSI                 |
| 12                    | MISO                 |
| 13                    | SCK                  |
| 5V                    | 5V                   |
| GND                   | GND                  |

<img src="assets/README/image-20241203142322453.png" alt="image-20241203142322453" style="zoom: 25%;" />





2，下载烧录工具https://github.com/avrdudes/avrdude/releases/download/v8.0/avrdude-v8.0-windows-x64.zip



3，下载串口固件 https://github.com/arduino/ArduinoCore-avr/blob/master/firmwares/atmegaxxu2/arduino-usbserial/Arduino-usbserial-atmega16u2-Uno-Rev3.hex



4，命令行烧录

```bash
# 先擦除芯片，然后设置熔丝位
./avrdude.exe -C avrdude.conf -v -patmega16u2 -cstk500v1 -PCOM19 -b19200 -e -Ulock:w:0x3F:m -Uefuse:w:0xF4:m -Uhfuse:w:0xD9:m -Ulfuse:w:0xFF:m
# -C avrdude.conf: 指定 avrdude 的配置文件路径。
# -v: 启用详细输出。
# -p atmega16u2: 指定目标芯片型号为 ATmega16U2。
# -c stk500v1: 使用 stk500v1 编程器协议。使用ArduinoUNO制作的ArduinoISP的型号就是stk500v1
# -P COM19: 指定使用的串口端口。
# -b 19200: 设置波特率为 19200。
# -e: 擦除芯片上的所有内容。
# -U lock:w:0x3F:m: 写入锁定位，设置为 0x3F。
# -U efuse:w:0xF4:m: 写入扩展熔丝位，设置为 0xF4。
# -U hfuse:w:0xD9:m: 写入高位熔丝位，设置为 0xD9。
# -U lfuse:w:0xFF:m: 写入低位熔丝位，设置为 0xFF。

# 最后烧录新的固件
./avrdude.exe -C avrdude.conf -v -patmega16u2 -cstk500v1 -PCOM19 -b19200 -Uflash:w:Arduino-COMBINED-dfu-usbserial-atmega16u2-Uno-Rev3.hex:i -Ulock:w:0x3F:m
# -U flash:w:...:i: 将指定的 HEX 文件写入芯片的闪存。
# Arduino-COMBINED-dfu-usbserial-atmega16u2-Uno-Rev3.hex 是要烧录的固件文件路径。
# -U lock:w:0x3F:m: 再次写入锁定位，确保固件被保护。
```



烧录日志

```bash
zx@DESKTOP-JP2PU76 MINGW64 /d/Downloads/avrdude-v8.0-windows-x64
$ ./avrdude.exe -C avrdude.conf -v -patmega16u2 -cstk500v1 -PCOM19 -b19200 -e -Ulock:w:0x3F:m -Uefuse:w:0xF4:m -Uhfuse:w:0xD9:m -Ulfuse:w:0xFF:m
Avrdude version 8.0
Copyright see https://github.com/avrdudes/avrdude/blob/main/AUTHORS

System wide configuration file is D:\Downloads\avrdude-v8.0-windows-x64\avrdude.conf

Using port            : COM19
Using programmer      : stk500v1
Setting baud rate     : 19200
AVR part              : ATmega16U2
Programming modes     : SPM, ISP, HVPP, debugWIRE
Programmer type       : STK500
Description           : Atmel STK500 v1
HW Version            : 2
FW Version            : 1.18
Topcard               : Unknown
Vtarget               : 0.0 V
Varef                 : 0.0 V
Oscillator            : Off
SCK period            : 0.0 us
XTAL frequency        : 7.372800 MHz

AVR device initialized and ready to accept instructions
Device signature = 1E 94 89 (ATmega16U2)
Erased chip

Processing -U lock:w:0x3F:m
Reading 1 byte for lock from input file 0x3F
in 1 section [0, 0]
Writing 1 byte (0x3F) to lock, 1 byte written, 1 verified

Processing -U efuse:w:0xF4:m
Reading 1 byte for efuse from input file 0xF4
in 1 section [0, 0]
Writing 1 byte (0xF4) to efuse, 1 byte written, 1 verified

Processing -U hfuse:w:0xD9:m
Reading 1 byte for hfuse from input file 0xD9
in 1 section [0, 0]
Writing 1 byte (0xD9) to hfuse, 1 byte written, 1 verified

Processing -U lfuse:w:0xFF:m
Reading 1 byte for lfuse from input file 0xFF
in 1 section [0, 0]
Writing 1 byte (0xFF) to lfuse, 1 byte written, 1 verified

Avrdude done.  Thank you.

zx@DESKTOP-JP2PU76 MINGW64 /d/Downloads/avrdude-v8.0-windows-x64
$ ./avrdude.exe -C avrdude.conf -v -patmega16u2 -cstk500v1 -PCOM19 -b19200 -Uflash:w:Arduino-COMBINED-dfu-usbserial-atmega16u2-Uno-Rev3.hex:i -Ulock:w:0x3F:m
Avrdude version 8.0
Copyright see https://github.com/avrdudes/avrdude/blob/main/AUTHORS

System wide configuration file is D:\Downloads\avrdude-v8.0-windows-x64\avrdude.conf

Using port            : COM19
Using programmer      : stk500v1
Setting baud rate     : 19200
AVR part              : ATmega16U2
Programming modes     : SPM, ISP, HVPP, debugWIRE
Programmer type       : STK500
Description           : Atmel STK500 v1
HW Version            : 2
FW Version            : 1.18
Topcard               : Unknown
Vtarget               : 0.0 V
Varef                 : 0.0 V
Oscillator            : Off
SCK period            : 0.0 us
XTAL frequency        : 7.372800 MHz

AVR device initialized and ready to accept instructions
Device signature = 1E 94 89 (ATmega16U2)
Auto-erasing chip as flash memory needs programming (-U flash:w:...)
specify the -D option to disable this feature
Erased chip

Processing -U flash:w:Arduino-COMBINED-dfu-usbserial-atmega16u2-Uno-Rev3.hex:i
Reading 7414 bytes for flash from input file Arduino-COMBINED-dfu-usbserial-atmega16u2-Uno-Rev3.hex
in 2 sections of [0, 0x3d33]: 59 pages and 138 pad bytes
Writing 7414 bytes to flash
Writing | ################################################## | 100% 8.41s
Reading | ################################################## | 100% 4.52s
7414 bytes of flash verified

Processing -U lock:w:0x3F:m
Reading 1 byte for lock from input file 0x3F
in 1 section [0, 0]
Writing 1 byte (0x3F) to lock, 1 byte written, 1 verified

Avrdude done.  Thank you.

```

