---
title: "Stepper STSPIN220"
date: 2021-02-20T22:56:28+05:30
draft: false
description : "library to use with stepper STSPIN220 in arduino ide"
featured : true
tags : [
    "arduino","platformIO","stepper","STSPIN220"
]
categories : [
    "Project","library"
]
series : ["Library"]
aliases : ["arduino-stepper-library"]
thumbnail : "images/Stepper_STSPIN220/logo.png"
---

# Stepper_STSPIN220-library Arduino

<h2> <u>LIBRARY DOWNLOAD</u> : <a href="https://github.com/pranav083/Stepper_STSPIN220-library"><i><b>LINK</b></i></a></h2>
This setup guide is made using the following hardware and software

**Hardware Components And Supplies :** 
1. Arduino IDE compatable board(e.g, Arduino Mega etc.)
2. STSPIN220 stepper driver
3. stepper motor with bipolar rated (< 1.2 Amps)
4. USB Cable to upload the code
5. Jumper Cable (Male to Male)
6. BreadBoard
   
**Software Used in the project :**
1. [PlatformIO](https://platformio.org/install/ide?install=vscode)
2. [Vscode](https://code.visualstudio.com/download)
   
### **Introduction**:
This is the library which was been developed for the 1/256 steps stepper motor driver mainly with the arduino. This stepper driver is a silent stepper driver which is used mailly in the operation of low current consuming stepper motors.  

**Note :** This stepper motor should be used with high current consuming stepper motor motor such as some high grades of NEMA 17 and above. 


<p align="center">
<img align="center" width="300" height="300" class="special-img-class" src="/images/Stepper_STSPIN220/2021-02-19-00-57-50.png" alt ="arduino" title ="stepper" >
</p>
/home/mighty/Documents/study/project/site/loiloi/static/images/Stepper_STSPIN220/2021-02-19-00-57-50.png

See the Datasheet: https://www.st.com/resource/en/datasheet/stspin220.pdf

The Stepper_STSPIN220 stepper driver is for Pololu stepper driver boards
and compatible clones. These boards use the Allegro Stepper_STSPIN220
stepper motor driver IC. (see Allegro website for datasheet)

This library diverges from others that are around, in that it
assumes that the MS1, MS2, MS3(DIR) and MS4(STEP) pins are connected to gpio
pins on the Arduino, allowing control over the microstepping
modes.

The Stepper_STSPIN220 is capable of microstepping down to 1/256 of a step, enabling fine control over the stepper motor. This fine control
can be used in, among other things, 3D printers.

This library provides an interface for setting the different
step modes, going from full step down to 1/256 step, using a
simple setter function, where the argument is 1, 2, 4, 8, 16, 32, 64, 128 or 256.

At just after the reset or wake up from standby the device is then only able to 
set the microstepping mode. 


| SL NO. | MODE4(DIR) | MODE3(STCK) | MODE2 | MODE1 | Step mode      |
| ------ | ---------- | ----------- | ----- | ----- | -------------- |
| 1.     | 0          | 0           | 0     | 0     | Full      step |
| 2.     | 0          | 1           | 0     | 1     | 1/2       step |
| 3.     | 1          | 0           | 1     | 0     | 1/4th     step |
| 4.     | 1          | 1           | 0     | 1     | 1/8th     step |
| 5.     | 1          | 1           | 1     | 1     | 1/16th    step |
| 6.     | 0          | 0           | 1     | 0     | 1/32nd    step |
| 7.     | 1          | 0           | 1     | 1     | 1/64th    step |
| 8.     | 0          | 0           | 0     | 1     | 1/128th   step |
| 9.     | 0          | 0           | 1     | 1     | 1/256th   step |

**Note:**
Lower delay values can be used in the microstepping mode.
Values as low as 25 usec can be used in the 1/256 mode
with some motors(for more info refer to the [datasheet](https://www.st.com/resource/en/datasheet/stspin220.pdf)). 

### **File content :**
*`Stepper_STSPIN220.cpp`* -- cpp file for using the Stepper_STSPIN220 stepper driver.  
*`Stepper_STSPIN220.h`*  -- header file for the declaration of fuction and class of the stepper driver.
 
#### Fuctions contain of library:

***Stepper_STSPIN220*** : Stepper class fuctions  
- `Stepper_STSPIN220(unsigned long motor_steps, int ms1_pin, int ms2_pin, int ms3_pin, int ms4_pin, int enable_pin)`

    This is the constructor fuction call at the initialization of file.

     1. `unsigned long motor_steps` : put the total no. of motor steps 
     2. `int ms1_pin` : define the pin from arduino board 
     3. `int ms2_pin` : define the pin from arduino board 
     4. `int ms3_pin` : define the pin from arduino board 
     5. `int ms4_pin` : define the pin from arduino board 
     6. `int enable_pin` : if pin is their then give the pin no.( default is -1)

- `void setDelay(unsigned long delay)`
This should be use to provide delay(`microsecond`). For stepper motor stepping.
    1. `unsigned long delay` : pass the delay in millisecond

- `void enable(bool val);`
can work only when the enable pin is actitaved
    1. `bool val` : pass `true` or `false` for enable or disable the motor

- `void setSpeed(long whatSpeed)`
set the speed of the stepper motor which is running jn the value of RPM.
    1. `long whatSpeed` : pass speed as parameter.

- `int version(void)`
Return the current software version 

- `void setDirection(bool direction)`
set the stepper direction in clock wise and anti-clockwise direction.
    1. `bool direction` : pass the direction as `true` and `false`

- `void step(unsigned long num_steps)`
take no. of steps as parameter and go in loop for that no. of steps.
    1. `static unsigned long num_steps`: provide the total no. steps to run.

- `void setStepMode(int stepMode)`
Only take power of 2 as in input e.g, 1, 2, 4, 8, 16, 32, 64, 128 or 256.
    1. `int stepMode` : pass the microstepping value that is described above.

### Testing :

For running the library in your system please run the `example.ino` file and have to make some adjustment in the file according to your pin connection and microstepping configuraion That is mainly the above file.

```cpp
int MODE_1 =  6;                    // Pin 6
int MODE_2 =  7;                    // Pin 7
int MODE_3 =  8;                    // Step Pin8
int MODE_4 =  9;                    // Dir Pin 9
int microstepping = 64;             // set microstepping mode   1, 2, 4, 8, 16, 32, 64, 128 or 256.
unsigned long MOTOR_STEPS = 200;    // Total no steps per revolution for motor in full step
unsigned long MOTOR_STEPS_TOTAL = MOTOR_STEPS * long(microstepping); // Total no. of motor steps
int EN     = -1;                    // no pin defined

int speed  = 10;                    //either set speed or give delay  
unsigned long stepdelay = 2000;     // Microseconds delay  // Higher# = more rapid changes between positions, lower = softer transitions

```

#### **Help in improving the guide :** 

Please if you find any useful improvement feel free to contact at : [@pranav083](https://twitter.com/pranav083) or comment below.