## PITB V2.0

![](PITB_V2_TOP.png)

### 1. Product introduction

Thanks for the great effort of the PCB designers [DFH](https://github.com/deepfriedheroin), [Armchair-Engineering](https://github.com/Armchair-Engineering), [kageurufu](Https://GitHub.com/kageurufu) and the other members from community to make this come true.

### 2. Hardware guide

#### 2.1 pinout

![](assets/PITB_V2_pinout_00.jpg)

<table>
   <tr><td>Features</td><td>PITB Pin</td><td>RP2040 Pin</td><td>Comment</td></tr>
   <tr><td rowspan="5">X-MOTOR(1)</td><td>X-Step</td><td>GPIO6</td><td></td></tr>
   <tr><td>X-DIR</td><td>GPIO5</td><td></td></tr>
   <tr><td>X-EN</td><td>GPIO20</td><td></td></tr>
   <tr><td>X-CS/PDN</td><td>GPIO1</td><td></td></tr>
   <tr><td>X-DIAG</td><td>GPIO7</td><td></td></tr>
   <tr><td rowspan="5">Y-MOTOR(2)</td><td>X-Step</td><td>GPIO13</td><td></td></tr>
   <tr><td>Y-DIR</td><td>GPIO23</td><td></td></tr>
   <tr><td>Y-EN</td><td>GPIO22</td><td></td></tr>
   <tr><td>Y-CS/PDN</td><td>GPIO21</td><td></td></tr>
   <tr><td>Y-DIAG</td><td>GPIO14</td><td></td></tr>
   <tr><td rowspan="3">TMC Driver SPI </td><td>MOSI</td><td>GPIO3</td><td></td></tr>
   <tr><td>MISO</td><td>GPIO4</td><td></td></tr>
   <tr><td>SCK</td><td>GPIO2</td><td></td></tr>
   <tr><td rowspan="2">End-stops</td><td>X-STOP</td><td>GPIO16</td><td></td></tr>
   <tr><td>Y-STOP</td><td>GPIO17</td><td></td></tr>
   <tr><td rowspan="3">FAN/RGB</td><td>FAN0</td><td>GPIO0</td><td></td></tr>
   </td><td>FAN1</td><td>GPIO18</td><td></td></tr>
   </td><td>RGB</td><td>GPIO19</td><td></td></tr>
   <tr><td rowspan="3">Temperature</td><td>T0（THERM0）</td><td>GPIO26</td><td>A 4.7kOhm 0.1% temperature sensor pull up resistor is used,PT1000 can be connected directly. For PT100, an amplifier board must be used.</td></tr>
   <td>T1（THERM1）</td><td>GPIO27</td><td>A 4.7kOhm 0.1% temperature sensor pull up resistor is used,PT1000 can be connected directly. For PT100, an amplifier board must be used.</td></tr>
   <td>T2（THERM2）</td><td>GPIO28</td><td>A 4.7kOhm 0.1% temperature sensor pull up resistor is used,PT1000 can be connected directly. For PT100, an amplifier board must be used.</td></tr>
   <tr><td rowspan="2">CAN</td><td>TX</td><td>GPIO8</td><td></td></tr>
   <tr><td>RX</td><td>GPIO9</td><td></td></tr>
   <tr><td rowspan="2">EEPROM I2C Pin-Out</td><td>SCL</td><td>GPIO25</td><td></td></tr>
   <tr><td>SDA</td><td>GPIO24</td><td></td></tr>
   <tr><td rowspan="3">SWD Debug</td><td>SWDIO</td><td>SWDIO</td><td>only used for debugging now and can be used for other purposes.</td></tr>
   <tr><td>SWCLK</td><td>SWCLK</td><td>only used for debugging now and can be used for other purposes.</td></tr>
   </td><td>RESET</td><td>#RUN</td><td></td></tr>
</table>

<h1>Firmware</h1>
At this time klipper only supports CAN not CAN FD, The jumper must be fitted between SEL and GND.
<img src="assets/PITB_V2_klipper_jumper.png">
<h2>Canboot/Katapult:</h2>
cd ~/CanBoot/<br>
make menuconfig<br>
<img src="assets/Katapult_firmware_PITBv2.png">
make clean<br>
make -j 4<br>
reboot into bootloader mode<br>
sudo make flash FLASH_DEVICE=2e8a:0003<br>
~/klippy-env/bin/python ~/klipper/scripts/canbus_query.py can0<br>
This should show a canboot device for you PITB the UUID is needed for klipper config and to flash the firmware<br>
<br>
<h2>Klipper Firmware</h2>
mkdir ~/printer_data/config/firmware<br>
cd ~/klipper<br>
#backup existying config for your current MCU<br>
cp -f ~/klipper/.config ~/printer_data/config/firmware/MCU.config<br>
make menuconfig<br>
<img src="assets/Klipper_firmware_PITBv2.png">
#backup config for PITB klipper firmware<br>
cp -f ~/klipper/.config ~/printer_data/config/firmware/pitb.config<br>
make clean<br>
make -j 4<br>
python3 ~/CanBoot/scripts/flash_can.py -i can0 -f ~/klipper/out/klipper.bin -u XXXXXXXXXXX<br>
<br>

<h1>Klipper Config:</h1>
<a href="PITB.cfg">PITB.cfg</a><br>
<a href="sensorless_homing.cfg">sensorless_homing.cfg</a><br>
