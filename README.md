# Robotiq 2-finger850 Gripper Driver
- This is a c++ driver for Robotiq 2-finger 850 gripper under Linux.
- For Windows user, just modify the COM port.
- **This package requried 3rd party library, [Boost](http://www.boost.org/). Be sure your Boost was well installed before building the package.**
- The following contains some basic control using modbus RTU communication instructions to control robotiq 2-finger 850 gripper.

### Maintainer
[Howard Chen](https://github.com/s880367), <howardchen.ece04g@g2.nctu.edu.tw>, National Chaio Tung University, Taiwan.

## Usage and install
- Just create ```/build``` and ```cmake..```, then ```make```
- **[Note]** Be sure to check your port ```/dev/ttyUSB0``` or ```/dev/ttyUSB1```or ```COM3``` etc.
- The program will **activate** and **open with full speed&force**. Then press ```enter```to **close** and **open** the gripper, ```q```to quit program.
- Because this program will access hardware device ```/dev/ttyUSBx```,which belongs to the ```dialout``` group. Therefore, run with ```sudo``` or if you are **non root user**, be sure to contact your root user to add you into the ```dialout group```.

### Troubleshooting
1. [Serial port terminal > Cannot open /dev/ttyS0: Permission denied](https://askubuntu.com/questions/210177/serial-port-terminal-cannot-open-dev-ttys0-permission-denied)
2. [How to allow a non-default user to use serial device ttyUSB0](https://askubuntu.com/questions/112568/how-do-i-allow-a-non-default-user-to-use-serial-device-ttyusb0)

## Connection Setup
The following table describes the connection requirements for controlling the Gripper using the Modbus RTU protocol.

| PROPERTY |VALUE |
| --------- | --------- |
|Physical Interface | RS-485 |
| Baud Rate | 115,200 bps |
| Data Bits | 8 |
|Stop Bits|1|
|Parity|None|
|Supported Functions|Read Holding Register(FC03) <br/>Press Single Register(FC06) <br/>Preset Multiple Register(FC16)<br/> Master read & write multiple registers (FC23)|
|Exception Response|Not Supported|
|Slave ID|0x0009|
|Robot output / Gripper input First Register|0x03E8(1000)|
|Robot input / Gripper output First Register|0x07D0(2000)|


## Modbus RTU protocol formate

- ### Request protocol
![](https://i.imgur.com/FqRcyjN.png)

- ### Response protocol
![](https://i.imgur.com/NnFiwNh.png)

## Modbus RTU command (ASCII)
**1. Activation**
 - Request : ```09 10 03 E8 00 03 06 00 00 00 00 00 00 73 30```
 - Response : ```09 10 03 E8 00 03 01 30```

**2. Read Gripper status until the activation is completed**
- Request : ```09 03 07 D0 00 01 85 CF```
- Response :
  - ```09 03 02 11 00 55 D5``` (The activation is not completed)
  - ```09 03 02 31 00 4C 15``` (The activation is completed)

**3. Close with full speed&force**

![](https://i.imgur.com/BGpGFU3.png)

- Request : ```09 10 03 E8 00 03 06 09 00 00 FF FF FF 42 29```
- Response : ```09 10 03 E8 00 03 01 30```

**4. Read status until closing is complete**
- Request : ```09 03 07 D0 00 03 04 0E```
- Response :
  - ```09 03 06 39 00 00 FF 0E 0A F7 8B``` (Closing is not completed)
  - ```09 03 06 B9 00 00 FF BD 00 1D 7C``` (Closing is completed)

**5. Open with full speed&force**
![](https://i.imgur.com/gjrxzuq.png)
- Request : ```09 10 03 E8 00 03 06 09 00 00 00 FF FF 72 19```
- Response: ```09 10 03 E8 00 03 01 30```

**6. Read status until opening is complete**
- Request: ```09 03 07 D0 00 03 04 0E```
- Response:
  - ```09 03 06 39 00 00 00 BB 10 30 E0```(Opening is not completed)
  - ```09 03 06 F9 00 00 00 0D 00 56 4C```(Opening is completed)


## Reference
- For more details, please check [Robotiq Official Website](http://support.robotiq.com/pages/viewpage.action?pageId=5963876)
