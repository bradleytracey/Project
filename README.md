# Ultrasonic Eyes

This read me contains the prerequisites for running the code, how to run it and how it works.

## Getting Started

To run the code, the following documents must be downloaded and placed in a file;

- Distance_readings.py
- Eye_designs.py
- Run_device.py


### Prerequisites

The code uses the Luma package to manipulate the LEDs. To install the package on the Raspberry-Pi, the latest version of the library can be downloaded directly from PyPi (https://pypi.org/project/luma.led_matrix/):

```
$ sudo -H pip install --upgrade luma.led_matrix 
$ sudo -H pip install --upgrade luma.core
```

“sudo -H” may be  omitted. If using python3 (recommended) then exchange

pip => pip3


### Running the code

Ensure you are in the folder where the three above files are situated and run the following:

```
$ python3 Run_device.py
```

### How the code works

The code is split into three sections. 

*Distance_reading.py*
This contains the definition ‘reading(sensor, TRIG, ECHO)’. When called, a 10us signal is sent down the TRIG GPIO and it listens in the ECHO pin for the length of the HIGH signal received. It then calculates and returns the value of distance measured by the HC-SR04 module. 


*Eye_designs.py* 
This file contains the different eyes designs. The LEDs we wish to turn on in the 8x8 matrix are put into a list ‘LEDSon’ (both rows and columns range from 0-7, eg (6,2) corresponded to the seventh column along and the third row down):
```
    LEDSon = [(2,0), (3,0), (4,0), (5,0),
        (1,1), (6,1),
        (1,2), (2,2), (3,2), (6,2),
        (0,3), (1,3), (2,3), (4,3), (7,3),
        (0,4), (1,4), (2,4), (3,4), (4,4), (7,4),
        (0,5), (2,5), (3,5), (7,5),
        (1,6), (6,6),
        (2,7), (3,7), (4,7), (5,7)]
```

The list LEDSon is then iterated through to turn each required LED on using the canvas tool from the luma.core package. This is repeated for each eye style and direction (left/right pointing eye). 

This file also contains several definitions, e.g. def set1(TRIG1, ECHO1, TRIG2, ECHO2, device).

 The function uses Distance_readings, reading() function to determine which direction the eyes should be pointing. It then selects an eye design to display, pointing in the correct direction and inserts a random single and double blink. 

*Run_device.py* 
Finally, this file takes the information from Eye_designs.py and puts the different eye sets into a loop, so that when a button is pressed, the different sets of eyes can be iterated:

```

	if (GPIO.input(button) == GPIO.HIGH):
		x = x + 1
	if x == 1: 
		eyes.set1(TRIG1, ECHO1, TRIG2, ECHO2, device)
	if x == 2: 
		eyes.set2(TRIG1, ECHO1, TRIG2, ECHO2, device)
	if x > 2:
		max7219.clear(device)
		x = 0
```

This part of the code is also where the intensity of the LED’s is defined. 

### Authors
* **Bradley Tracey** //
**Amy Teasdale** 

