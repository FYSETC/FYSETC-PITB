Tempory files while getting support on the SPI errors

Checked with multimeter that all ground and voltages are showing, as well as the canbbus fully connected with right termination
At the moment system is being fed with 2x 24v supplys as i am working on the 48v upgrade. Both 24v and motor power(48v) are being fed 24v.

System can see main controla board with usb2can, PITB and EBB36 and all comunicating using thew can bus
System is reporting temptures, MCU load ect for all MCU's

My config and logfile is attatched

system appares working untill you try to do anything with X or Y steppers connected to PITB.
Sometimes a single command may seem to work to SET_TMC_FIELD but only if it is set to -64

Video attached as requested
