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
   <tr><td rowspan="3">SWD Debug</td><td>SWDIO</td><td>SWDIO</td><td>25</td><td>only used for debugging now and can be used for other purposes.</td></tr>
   <tr><td>SWCLK</td><td>SWCLK</td><td>24</td><td>only used for debugging now and can be used for other purposes.</td></tr>
   </td><td>RESET</td><td>#RUN</td><td>26</td><td></td></tr>
</table>
## Firmware

Klipper:<br>
[mcu PITB]<br>
canbus_uuid: XXXXXXXXXXXX<br>
<br>
[stepper_x]<br>
step_pin:PITB:gpio6<br>
dir_pin:PITB:!gpio5<br>
enable_pin:PITB:!gpio20<br>
#endstop_pin:PITB:^!gpio16<br>
endstop_pin: tmc2130_stepper_x:virtual_endstop<br>
homing_retract_dist: 20<br>
rotation_distance: 40<br>
position_endstop: 0<br>
position_max: 209<br>
homing_speed: 150.0<br>
second_homing_speed: 10.0<br>
<br>
[TMC5160 stepper_x]<br>
cs_pin:PITB:gpio1<br>
spi_software_sclk_pin:PITB:gpio2<br>
spi_software_mosi_pin:PITB:gpio3<br>
spi_software_miso_pin:PITB:gpio4<br>
diag1_pin:PITB:!gpio7<br>
driver_SGT: -64  # -64 is most sensitive value, 63 is least sensitive<br>
#stealthchop_threshold: 999999<br>
interpolate: False<br>
<br>
[stepper_y]<br>
step_pin:PITB:gpio13<br>
dir_pin:PITB:gpio23<br>
enable_pin:PITB:!gpio22<br>
#endstop_pin:PITB:^!gpio17<br>
endstop_pin: tmc2130_stepper_y:virtual_endstop<br>
homing_retract_dist: 20<br>
rotation_distance: 40<br>
position_endstop: 210<br>
position_max: 210<br>
homing_speed: 150.0<br>
second_homing_speed: 10.0<br>
 <br>
[TMC5160 stepper_y]<br>
cs_pin:PITB:gpio21<br>
spi_software_sclk_pin:PITB:gpio2<br>
spi_software_mosi_pin:PITB:gpio3<br>
spi_software_miso_pin:PITB:gpio4<br>
diag1_pin:PITB:!gpio14<br>
driver_SGT: -64  # -64 is most sensitive value, 63 is least sensitive<br>
#stealthchop_threshold: 999999<br>
interpolate: False<br>
<br>
[fan_generic PITB_FAN0]<br>
pin:PITB:gpio0<br>
max_power: 1.0<br>
kick_start_time: 0.5<br>
off_below: 0.10<br>
<br>
[fan_generic PITB_FAN1]<br>
pin:PITB:gpio18<br>
max_power: 1.0<br>
kick_start_time: 0.5<br>
off_below: 0.10<br>
<br>
[neopixel PITB_LED]<br>
pin:PITB:gpio19<br>
color_order: GRBW<br>
initial_RED: 0.0<br>
initial_GREEN: 0.0<br>
initial_BLUE: 0.0<br>
initial_WHITE: 0.0<br>
<br>
#[temperature_sensor PITB_TEMP0]<br>
#sensor_type: ATC Semitec 104NT-4-R025H42G<br>
#sensor_pin:PITB:gpio26<br>
#min_temp: -50<br>
#max_temp: 300<br>
<br>
#[temperature_sensor PITB_TEMP1]<br>
#sensor_type: ATC Semitec 104NT-4-R025H42G<br>
#sensor_pin:PITB:gpio27<br>
#min_temp: -50<br>
#max_temp: 300<br>
<br>
#[temperature_sensor PITB_TEMP2]<br>
#sensor_type: ATC Semitec 104NT-4-R025H42G<br>
#sensor_pin:PITB:gpio28<br>
#min_temp: -50<br>
#max_temp: 300<br>
