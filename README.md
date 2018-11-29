# Ultrasonic Eyes

This read me contains the prerequisites for running the code, how to run it and how it works.

## Getting Started

You will first need to download the following documents and place them in the same folder from which you wish to run the code: 

- Distance_readings.py
- Eye_designs.py
- Run_device.py


### Prerequisites

The code uses the Luma package to manipulate the LEDs. This package must first be installed on the Raspberry Pi and u need to install the software and how to install them. 

Proceed to install the latest version of the library directly from PyPi (https://pypi.org/project/luma.led_matrix/): 

```
$ sudo -H pip install --upgrade luma.led_matrix 
$ sudo -H pip install --upgrade luma.core
```

“sudo -H” may be  omitted. Uf Using python3 (recommended) then exchange

pip => pip3


### Running the code

Ensure you are in the folder where the three above files are situated and run the following:

```
$ python3 Run_device.py
```

### How the code works

The code is split into three sections. 

*Distance_reading.py*
This contains the definition ‘reading(sensor, TRIG, ECHO)’. When called, a 10us signal is sent down the TRIG GPIO and it listens in the ECHO pin for the length of the HIGH signal received. It then calculated and returns the value of distance. 


*Eye_designs.py* 
This contains the different eyes designs. The LEDs we wish to turn on the 8x8 LED matrix are put into a list ‘LEDSon’:
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

The list of LEDs is then iterated to turn each on individually using the canvas tool from the luma.core package. This is done individually for each eye style and direction (point eyes left, point right). 

This also contains definition, def set1(TRIG1, ECHO1, TRIG2, ECHO2, device): etc. The function implements Distance_readings, reading() function to determine which direction the eyes should be pointing. It then selects an eye design to display, pointing in the correct direction and inserts a random single and double blink. 

*Run_device.py* 
Finally, this takes the information from Eye_designs.py and puts the different eye sets into a loop, so that when a button can be pressed, the different sets of eyes can be iterated:

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

This part of the code is also where the intensity is set. 

### Authors
**Bradley Tracey**
**Amy Teasdale** 

