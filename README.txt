﻿Most commercial water/fluid reward systems used in research settings cost a few hundred dollars (or more) and aren't easily modified. This is a fairly quick hack, but it does the job well and won't cost you >$100.. much less if you already have some of the components sitting around. Using a peristaltic pump is preferred, as the motor never actually comes into contact with the fluid itself.

----------------------------------------------------------------
---------------------------EQUIPMENT----------------------------
----------------------------------------------------------------
Common Equipment (All Versions):
- Peristaltic Liquid Pump (12VDC / 300mA) with Silicone Tubing (max flow 100mL/min) [ http://www.adafruit.com/products/1150 ]
- 12VDC Adapter (old D-Link wall adapter in my case) to power the motor
- Either a Raspberry Pi or an Arduino (see below)
- Some way of driving the motor (Motor Shield, Gertboard, L293D, or Power Transistor -- see below)
- Momentary Push Button/Switch for manual fluid delivery (optional)

--------- *** I use a Raspberry Pi w/the L293D IC.
Equipment Used for Arduino or Raspberry Pi w/L293D Version:
- L293D[ http://www.ti.com/lit/ds/symlink/l293d.pdf ; http://www.adafruit.com/products/807 ]

---------
Equipment Used for Raspberry Pi/Gertboard Version:
- Raspberry Pi Model B [ http://www.newark.com/jsp/search/productdetail.jsp?sku=43W5302 ]
- Gertboard [ http://www.newark.com/jsp/search/productdetail.jsp?sku=46W9829 ]

---------
Equipment Used for Arduino/SeeedStudio Motor Shield Version:
- Arduino Duemilanova w/ATMEGA328 [ http://arduino.cc/en/Main/arduinoBoardDuemilanove ]
- Seeed Studio Motor Shield [ http://www.seeedstudio.com/depot/motor-shield-p-913.html ] 



----------------------------------------------------------------
-------------------ARDUINO OR PI W/L293D CHIP-------------------
----------------------------------------------------------------
As a starting off point, the DC motor tutorials at Adafruit can be used for either the Arduino or the Raspberry Pi. The Adafruit Raspberry Pi tutorial makes use of their Occidentalis distro, which has a custom PWM kernel module included. This might make your life easier, but I prefer Raspbian and wiringPi (below).
Raspberry Pi: http://learn.adafruit.com/adafruit-raspberry-pi-lesson-9-controlling-a-dc-motor/overview
Arduino: http://learn.adafruit.com/adafruit-arduino-lesson-15-dc-motor-reversing/

L293D pinout can be found here: http://www.ti.com/lit/ds/symlink/l293d.pdf

I used the Raspberry Pi with the L293D, with a base (updated) Raspbian install.
1a. Install wiringPi either w/apt-get from the command line:
>> sudo apt-get update
>> sudo apt-get install python-dev python-pip
>> sudo pip install wiringpi
1b. Or install wiringPi2 from the git repository (this is what I did):
>> sudo apt-get install git-core
>> git clone git://github.com/WiringPi/WiringPi2-Python.git
>> cd wiringPi2-Python
>> sudo python setup.py install
2. The L293D_pump_code_wiringPi.py script works similar to the Gertboard script.
3. The breadboard version can be seen in the L293D*.jpg pictures & the Fritzing wiring diagram can be found in L293D_wiring_diagram.png.
4. The schematic and soldered versions can also be seen in their respective pictures.



----------------------------------------------------------------
--------------ARDUINO/SEEED MOTOR SHIELD VERSION----------------
----------------------------------------------------------------
I used an older arduino (duemilanova) I had sitting around, but any version should be fine. If you use the Seeed motor shield you'll need a compatible pinout -- the duemilanova or the uno both work well. I wouldn't specifically recommend the motor shield, it's overkill for this, but it's cheap/simple and if you have one laying around it works fine. If you do go for this particular motor shield, the wiki has good information: [ http://www.seeedstudio.com/wiki/Motor_Shield_V1.0 ].

Connect your DC power adapter to the motor shield Vs / Gnd pins, and connect the motor to the M1+ / M1- pins. Connect the motor shield to the arduino. Upload arduino code via USB. If you leave the J6 jumper connected, you don't need to provide a separate power source to the arduino -- it will pull power from the motor shield. If you want a manual "push button to dispense fluid" feature, you can pick up a button module from Seeed and connect it via their Grove system to make life easy, but I used a 10kOhm resistor and an old button switch I had laying around.

			Switch
			  |
			  |------------
			  |	       |
			  |	       |
			  |           / \
			 5V	     /	 \
			       10kOhm	  Arduino Pin #5			
			      Resistor
				 |
				 |
				Gnd

The physical assembly was done using odd metal pieces I had laying around. I'm not particularly proud of it, but it works fine (it doesn't really need a physical assembly to be functional). See pictures for reference.

	
	
----------------------------------------------------------------
-----------------RASPBERRY PI/GERTBOARD VERSION-----------------
----------------------------------------------------------------
A gertboard is overkill for this, but again, I already had it sitting around. :) I used a raspberry pi model B, with a pre-assembled gertboard to drive the motor. If you go the gertboard route, their manual is valuable: [ http://www.element14.com/community/docs/DOC-51727/l/assembled-gertboard-user-manual-with-schematics ]. 

0. Connect your Gertboard to your Raspberry Pi.
1. Add one jumper wire between Gertboard pins (see pictures for reference): GP17 <-> MOTB
2. Add another jumper wire between Gertboard pins: GP18 <-> MOTA
3. Connect pump motor (+) to screw terminal MOTA and pump motor (-) to screw terminal MOTB.
4. Connect the positive lead from your 12VDC adapter to screw terminal MOT+ and neutral to screw terminal ground.
5a. Install wiringPi either w/apt-get from the command line:
>> sudo apt-get update
>> sudo apt-get install python-dev python-pip
>>sudo pip install wiringpi
5b. Or install wiringPi from the git repository:
>> sudo apt-get install git-core
>> git clone git://git.drogon.net/wiringPi
>> cd wiringPi
>> ./build
6. Play with the other Gertboard examples (including motor ones) if you are so inclined:
>> get http://raspi.tv/download/GB_Python.zip



----------------------------------------------------------------
--------------------------GENERAL NOTES-------------------------
----------------------------------------------------------------
You can use pulse width modulation w/the motor to speed up or slow down the flow rate; if you connect the motor the other way it will move fluid the other direction. It uses 3/16″ (4.7mm) outer diameter silicone tubing, which connects nicely to a metal drinking tube pulled out of a rodent water bottle.







