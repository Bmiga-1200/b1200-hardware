# WIP
# B1200
Modern Amiga 1200 clone implementation using Cyclone V FPGA

## Features
The hardware design supports these features:
1. USB-C PD Sink / TCPC
3. USB-C DP Sink (4-lane Alt Mode)
   * DisplayPort Output
   * DisplayPort Retimer
4. USB-C 2.0 End point / Hub
   * USB Keyboard
   * USB Mouse
   * UART to FPGA Debug port
   * UART to ESP32 Programming port / bootstrap
5. KVM to switch Monitor/Keyboard/mouse between USB-C upstream or FPGA USB OTG
6. DVI-A output
   * DVI to FPGA
   * VGA to ESP32   
7. ESP32 as the embedded controller
   * KVM Switch control
   * Fan Speed Control
   * A500 Keyboard scanner
   * USB HID
   * VGA (64 color) display for boot time messages
   * RTC Emulation on I2C
   * SLiRP Modem Emulation on UART
8. FPGA I2S to analog 3.5mm audio jack output
9. DDR RAM to FPGA expansion bus
10. Cyclone V FPGA on the draughtboard

## Hardware Pin assignments:

### EC (ESP32) Functions and pin assignments
@o: output
@i: Input
3v3: +3.3V logic
1v8: +1.8V logic

* Boot strap (flash): IO0@i, EN@i, U0RXD@i3v3, U0TXD@o3v3
* TCPC
  - Negotiate SINK 5V@3A + 4-lane DP Alt Mode
* I2C Controller: IO21/SDA, IO38/SCL
* DVI-A/VGA output:
  - Red[0,1] = [IO10,IO11]@o3v3
  - Green[0,1] = [IO45,IO40]@o3v3
  - Blue[0,1] = [IO46,IO39]@o3v3
  - HSYNC = IO47@o1v8
  - VSYNC = IO48@o1v8
* DDR
  - Power Good: IO2@o3v3
  - I2C same pin as controller
* WiFi SLiRP modem, UART1
  - TxD = U1TXD@o3v3
  - RxD = U1RXD@i3v3
* KVM
  - User Switch: IO3@i falling edge
  - Port selection: IO5@o3v3, L = USBC DP, H = FPGA DP
* FPGA
  - Temp sensor interupt alaram IO4@i3v3 falling edge
  - Cooling Fan: SENS = IO7@i3v3, PWM = IO8@o3v3
  - I2C peripheral: IO41/SCL, IO42/SDA
* Expansion port:
  - ADC1_CH8
* A500 Keyboard:
  - CLK = IO12@o3v3
  - DATA = IO13@i3v3
  - RST = IO14@o3v3
  - LED STATUS = IO15@o3v3
  - LED INUSE = IO16@o3v3

### FPGA (Cyclone V) Functions and pin assignments

* DP on HDMI Flext

|Net|FPGA Pin|
|-|-|
|DP0-|AD11|
|DP0+|AD10|
|DP1-|AH5|
|DP1+|AH6|
|AUX-|AE7|
|AUX+|AF6|
|HPD |AF11|

* DP on GPIO_1 header

|Net |FPGA Pin|
|-|-|
|DP0-|AA15|
|DP0+|Y15|
|DP1-|AF28|
|DP1+|AF27|
|DP2-|AH27|
|DP2+|AG28|
|AUX-|AD26|
|AUX+|AE25|
|HPD |AC24|
|<span style="text-decoration:overline">LP</span>  |AG26|

* DVI-D

|Net|FPGA Pin|
|-|-|
|D0+|AG25|
|D0-|AF25|
|D1+|AH24|
|D1-|AG24|
|D2+|AG23|
|D2-|AF23|
|CK+|AH23|
|CK-|AH22|
|HPD|AG21|

* SNAC on HDMI

|Pin|FPGA Pin|
|-|-|
|1|AH9|
|2|AG11|
|3|AF15|
|4|AH11|
|5|AH12|
|6|AG16|
|7|AF17|
|8|AD4|

* I2S for Line Out

|Net|FPGA Pin|
|-|-|
|DIN|AC4|
|LRCK|AG8|
|BCK|AG10|
|<span style="text-decoration:overline">MUTE</span>|AH8|

* SliRP UART

|Net|FPGA Pin|
|-|-|
|RXD|U9|
|TXD|V10|

* I2C Controller

|Net|FPGA Pin|
|-|-|
|SDA|AG9|
|SCK|U14|

* Input Switches

|Net|FPGA Pin|
|-|-|
|OSD|AF13|
|RST|AG13|
|USER|U13|

* LED outputs

|Net|FPGA Pin|
|-|-|
|PWR|AA23|
|USR|AE26|
|DSK|Y16|


# Thanks/Credit

Some sections of the hardware design were inspired by the following projects.
- [Terasic](https://www.terasic.com.tw/en/) DE10-Nano
- [QMTECH_Cyclone_V_SoC_KFB](https://github.com/ChinaQMTECH/QMTECH_Cyclone_V_SoC_KFB)
- [Hardware_MiSTer: Daughter boards](https://github.com/MiSTer-devel/Hardware_MiSTer) Hardware addons for MiSTer
- [PicoDVI](https://github.com/Wren6991/PicoDVI): Bitbanged DVI on the RP2040 Microcontroller
- [SNAC](https://github.com/blue212/SNAC): Accessory converter for MiSTer
- [DE10Jamma](https://github.com/MisterJamma/DE10Jamma) JAMMA interface for DE10
- [DPSwitch](https://github.com/jorticus/DPSwitch): a simple KVM-style device
- [amigakb](https://github.com/tkoecker/amigakb): Arduino Amiga keyboard interface
- [amiga-keyboard](https://github.com/hekkelek/amiga-keyboard): Mechanical Replacement Keyboard for Amiga 500 and 1200
- [Amiga1200-Plus](https://bitbucket.org/jvandezande/amiga-1200/): a re-implementation of the Amiga 1200 board
- [Amiga-Keyboard](https://github.com/solarmon/Amiga-Keyboard/): map of Commodore Amiga keyboards


# License

The hardware files in this repository are released under Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)

[![licensebuttons by-nc-sa](https://licensebuttons.net/l/by-nc-sa/4.0/88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0)
