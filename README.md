## Tutorial:  

 https://www.geeetech.com/blog/2016/09/how-to-use-3dtouch-auto-leveling-sensor-on-your-geeetech-prusa-i3/

 https://www.geeetech.com/Documents/3DTouch%20auto%20leveling%20sensor%20%20User%20Manual.pdf


 ## Firmware:  

 Be aware there are two different types of GT2560 Boards. The old one with the servo pins on top and the newer one A+ which I am using.

The page to download the different projects for the different boards

 https://www.geeetech.com/forum/viewtopic.php?f=20&t=17046

 When you download the software, open it in arduino ide, try to compile a error message shows up:

 Error on "Struct fpos_t {" ...
 
	this means, that the developer was to lazy to rename the the header and a library uses the same name.
	just rename ALL structs with something like fpos_t2 and the error should be gone


## Backup board:  

Install avrdude with homebrew.
To download type :r: ...read
To upload tpye :w: ...write

#### Download:

avrdude -c arduino -P /dev/ttyACM0 -b 115200 -p m328p -U flash:r:SaveFlash.hex:i

#### Upload: 

avrdude -c arduino -P /dev/ttyACM0 -b 115200 -p m328p -U flash:w:SaveFlash.hex:i

#### Definition: 
avrdude \
    -c programmer type 
    -P connection port 
    -b override RS-232 baudrate
    -p partno, AVR device
    -U memtype>:r:file_name[:format]

If used directly from from the arduino IDE archive also add:
-C path-to/arduino-V/hardware/tools/avr/etc/avrdude.conf

https://arduino.stackexchange.com/questions/179/is-there-any-way-to-download-a-sketch-from-an-arduino


## 3d touch autbed leveling: 

#### Invert Achse when goes to wrong direction
Invert the stepper direction. Change (or reverse the motor connector) if an axis goes the wrong way.

    #define INVERT_X_DIR false
    #define INVERT_Y_DIR true
    #define INVERT_Z_DIR false

#### Set preheat abs/pla 
    #define PLA_PREHEAT_HOTEND_TEMP 190
    #define PLA_PREHEAT_HPB_TEMP 55
    #define PLA_PREHEAT_FAN_SPEED 255   // Insert Value between 0 and 255

#### set the rectangle in which to probe
ifdef AUTO_BED_LEVELING_GRID

    #define LEFT_PROBE_BED_POSITION 30
    #define RIGHT_PROBE_BED_POSITION 200
    #define BACK_PROBE_BED_POSITION 147
    #define FRONT_PROBE_BED_POSITION 20

####to change the Z Offset default 
in the Configuration.h uncomment the two defines 

    #define EEPROM_SETTINGS
    #define EEPROM_CHITCHAT

then, when you change for example the z_offset in the Control, Motion Z_Offset ...
    you can now safe the changes in the main menu with `Store memory`
    it is also possible the load the backup with `Load Memory` or get the firmware values back with `Restory failsafe`
    but than all the firmware values getting written in the EEPROM

this also works for the Temperature settings with PLA and ABS

#### measuring

the achses where the printer is running on should have the same distance
https://github.com/MarlinFirmware/Marlin/issues/5578#:~:text=%40eduardoscamargo%20I%20have%20actually%20gotten%20a%20few%20direct%20e%2Dmails%20with%20this%20question.%20I%20have%20made%20a%20video%20descriptively%20explaining%20what%20I%20meant%20by%20%22twisted%22%20and%20have%20uploaded%20it%20here%20for%20anyone%20who%20may%20be%20having%20the%20same%20struggle%20I%20was.%20CLICK%20HERE%20TO%20GO%20TO%20EXPLANATION%20VIDEO

take a measuring stick or something to measure and be sure that every distance is in water (same distance) 
tooks me hours to figure out, that the x axis was crooked