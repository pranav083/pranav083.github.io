---
title: "LVGL hardware button setup"
date: 2021-02-15T14:27:35+05:30
draft: false
description : "showing setup of esp32 lvgl and hardware button"
featured : true
tags : [
    "esp32","lvgl","platformIO","button"
]
categories : [
    "Project",
]
series : ["Setup Guide"]
aliases : ["lvgl-arduino-esp32-setup"]
thumbnail : "images/lvgl_hardware_button_setup/logo.png"
---
# Arduino Esp32 LVGL lv_arduino Hardware_button st7735s

This setup guide is made using the following hardware and software

**Hardware Components And Supplies :-**
1. [Esp32 Dev Module](https://components101.com/sites/default/files/component_pin/ESP32-Pinout.png)
2. ST7735s 1.8" TFT Display 128 x 160
3. USB Cable to upload the code
4. Jumper Cable (Male to Male)
5. BreadBoard
6. Push button or Capacitive touch button

**Software Used in the project :**
1. [PlatformIO](https://platformio.org/install/ide?install=vscode)
2. [Vscode](https://code.visualstudio.com/download)
3. [esp32 Framework:](https://github.com/espressif/arduino-esp32) Arduino
4. [lvgl/lv_arduino@2.1.5](https://github.com/lvgl/lv_arduino/releases/tag/2.1.5)
5. [bodmer/TFT_eSPI@^2.3.59](https://github.com/Bodmer/TFT_eSPI/releases/tag/2.3.59)
6. [greiman/SdFat@^2.0.4](https://github.com/greiman/SdFat/releases/tag/2.0.4)
7. [lvgl/lvgl@^7.10.0](https://github.com/lvgl/lvgl/releases/tag/v7.10.0)
8. [lvgl/lv_examples@^7.10.0](https://github.com/lvgl/lv_examples/releases/tag/v7.10.0)

It is easier to download the required library from the platformIO library section from its home page.

**Note:-** It is recommended to go with that version of the library as it is tested on the system and it is working properly. And please go through the Display SETUP guide once.
<!-- put the link of setup guide lvgl esp32 -->
#### Introduction:

The main aim of this project is to able to run the LVGL library on the st7735s 128 x 160 TFT display with using esp32 and configure multiple button input from the different GPIO of the ESP32 and pass that data command to the screen fuctionality. I am driving my system in the landscape mode you can go with portrait mode also but you to configure the setting properly. If faces any problen on the screen setup go to the previous guide of lvgl screen [**Setup**](.././lvgl_esp32_setup).
In this setup , I have put total of 8 button from the esp32 which are both input and touch configurable in the esp32 board

**Earlier Tested** : 1. Earlier I tested the blog for setting and registring the button on the lvgl, from this [Guide](https://blog.lvgl.io/2019-01-08/hardware-button) **some functions depriciated** in the guide. 
1. But unforunately that guide have setup defined on the stm32 board and also itis for the older version of the lvgl and some of the fuction are depriciated in the newer version of the lvgl.

### putting the Setup 

**Step 1**:Create a new project in platformIO and Do the connection of the esp32 with Capacitive touch button are as follows:  

<p align="center">
<img align="center" width="500" height="300" class="special-img-class" src="/images/lvgl_hardware_button_setup/16-02-2021-16-10-44.png" alt ="ST7735s" title ="Capacitive Touch sensor"  >  
</p>  

$$\text{Capacitive Touch sensor}$$ 

<p align="center">
<img align="center" width="500" height="300" class="special-img-class" src="/images/lvgl_hardware_button_setup/16-02-2021-16-50-49.png" alt ="ST7735s" title ="Capacitive Touch sensor setup"  >  
</p>

$$\text{Capacitive Touch sensor setup}$$ 

<p align="center">
<img align="center" width="500" height="300" class="special-img-class" src="/images/lvgl_esp32_setup/11-02-2021-13-45-18.png" alt ="ESP32" title ="ESP32" >
</p>

$$\text{ESP32 dev Board }$$ 

#### System Connections:


| Sl No. | Capacitive Touch Signal pin | ESP32 | Description             |
| ------ | --------------------------- | ----- | ----------------------- |
| 1.     | Button_1                    | 27    | Capacitive touch button |
| 2.     | Button_2                    | 14    | Capacitive touch button |
| 3.     | Button_3                    | 12    | Capacitive touch button |


**Step 2:**  Now do the following setup in the downloaded library as in my case i used PlatformIO so i will go to :  
`{your platformIO project name}/.pio/libdeps/esp32dev/TFT_eSPI/User_Setup.h`  
and for the Arduino IDE go to `/home/{USERNAME}/Arduino/libraries/TFT_eSPI/User_Setup.h`  

So either you can comment everything their and put these lines or read the file uncomment these changes in the `User_Setup.h`. 

**Step 2:** Now go to the `{your platformIO project name}/.pio/libdeps/esp32dev/lv_arduino/lv_conf.h` and for the Arduino IDE go to `/home/{USERNAME}/Arduino/libraries/.pio/libdeps/esp32dev/lv_arduino/lv_conf.h`  and do the following changes :
```cpp
/* Input device read period in milliseconds */
#define LV_INDEV_DEF_READ_PERIOD          30            // for button till 20ms is recommended
/* Long press time in milliseconds.
 * Time to send `LV_EVENT_LONG_PRESSED`) */
#define LV_INDEV_DEF_LONG_PRESS_TIME      400           // if long press required
/* Repeated trigger period in long press [ms]
 * Time between `LV_EVENT_LONG_PRESSED_REPEAT */
#define LV_INDEV_DEF_LONG_PRESS_REP_TIME  100

// enable log settings
#define LV_USE_LOG      1
#  define LV_LOG_LEVEL     LV_LOG_LEVEL_INFO            // easy for debugging 
#  define LV_LOG_PRINTF   1
```

**Step 3:** Now go to the `{your platformIO project name}/.pio/libdeps/esp32dev/lvgl/examples/porting` folder and for the Arduino IDE go to `/home/{USERNAME}/Arduino/libraries/.pio/libdeps/esp32dev/lvgl/examples/porting`  and do the following changes :
* and copy the file name `lv_port_indev_template.c` as `lv_port_indev.c` and `lv_port_indev_template.h` as `lv_port_indev.h` in the folder name `lv_port_indev` in your local lib folder in the PlatformIO or in the skechbook folder of the Arduino IDE.
* Now open the `lv_port_indev.h` file and remove with following lines: 
  ```cpp
    /**
    * @file lv_port_indev.h
    *
    */

    /*Copy this file as "lv_port_indev.h" and set this value to "1" to enable content*/
    #if 1

    #ifndef LV_PORT_INDEV_H
    #define LV_PORT_INDEV_H

    #ifdef __cplusplus
    extern "C" {
    #endif

    /*********************
    *      INCLUDES
    *********************/
    /*********************
    *      DEFINES
    *********************/
    #define BUTTON_1         27      // GPIO from esp32 dev module
    #define BUTTON_2         14      // GPIO from esp32 dev module
    #define BUTTON_3         12      // GPIO from esp32 dev module
    /**********************
    *      TYPEDEFS
    **********************/
    typedef struct 
    {
        const char pin_name[15];
        int8_t pin_no;
        int8_t button_no;
        volatile bool pin_state;
        int8_t pin_mode;
        bool pin_last_state;
    }buttons_c;

    /**********************
    * GLOBAL PROTOTYPES
    **********************/
    void lv_port_indev_init(void);
    /**********************
    *      MACROS
    **********************/

    #ifdef __cplusplus
    } /* extern "C" */
    #endif

    #endif /*LV_PORT_INDEV_TEMPL_H*/

    #endif /*Disable/Enable content*/

  ```
* Now open the `lv_port_indev.c` file and add the following lines: 
  ```cpp
    //enable the content 
    #if 1

    /*********************
    *      INCLUDES
    *********************/    
    // include the header file
    #include <Arduino.h>
    # include <lv_examples.h>
    #include "lv_port_indev.h"
    /*********************
        DEFINES
    *********************/
    buttons_c buttons [] = {
        {.pin_name = "BUTTON_1 "  , .pin_no = BUTTON_1  , .button_no = 1},
        {.pin_name = "BUTTON_2 "  , .pin_no = BUTTON_2  , .button_no = 2},
        {.pin_name = "BUTTON_3 "  , .pin_no = BUTTON_3  , .button_no = 3}
    };    
    // in the fuction void lv_port_indev_init(void)
    /*Assign buttons to points on the screen change according to your preference*/
    static const lv_point_t btn_points[(sizeof(buttons) / sizeof(buttons_c))] = {
        { 80, 25},   /*Button 1 at cordinate on screen -> x:80; y:10*/
        { 80, 85},   /*Button 2 at cordinate on screen -> x:80; y:30*/
        { 80, 50},   /*Button 3 at cordinate on screen -> x:80; y:50*/
    }    
    // in the fuction static void button_init(void)
    /* Initialize your buttons */
    static void button_init(void)
    {
        uint8_t i;
        for(i = 0; i < sizeof(buttons) / sizeof(buttons_c); i++) 
        {
            pinMode(buttons[i].pin_no, buttons[i].pin_mode = INPUT);
            buttons[i].pin_state = digitalRead(buttons[i].pin_no);
            buttons[i].pin_last_state = buttons[i].pin_state;  
            buttons[i].pin_last_state = -1;
        }
    }   
    // in the fuction static int8_t button_get_pressed_id(void)
    /*Get ID  (1- 3) of the pressed button*/
    static int8_t button_get_pressed_id(void)
    {
        uint8_t i;    
        for(i = 0; i < sizeof(buttons) / sizeof(buttons_c); i++) 
        {
            buttons[i].pin_last_state = buttons[i].pin_state;
            buttons[i].pin_state = digitalRead(buttons[i].pin_no);
            if(button_is_pressed(buttons[i].pin_state))
                return buttons[i].button_no;                // return button no.
        }
        /*No button pressed*/
        return -1;
    }

    /*Test if `id` button is pressed or not*/
    static bool button_is_pressed(uint8_t id)
    {
        if(id)
            return true;
        return false;
    }
  ```

**Step 4:** Now open a new file in PlatformIO from the `src` folder like `main.cpp` or in the ArduinoIDE open up a main file which contain the `void setup()` fuction and do these changes: 
```cpp
    // include the following library
    #include <Arduino.h>
    #include <lvgl.h>
    #include <main.h>
    #include <TFT_eSPI.h>
    #include <lv_examples.h>
    #include "../lib/lv_port_indev/lv_port_indev.h"    
```
And in `void setup()` fuction add this after display driver initialization :
```cpp
    lv_disp_drv_register(&disp_drv);

    /* Try an example from the lv_examples repository
		 * https://github.com/lvgl/lv_examples*/
    lv_port_indev_init();   // add this line 
    lv_ex_btn_1();          // calling from lv_example 
```


**Step 6:** Now upload the program to your ESP32. Here is the content of   `platformio.ini` file:

```cpp
    [env:esp32dev]
    platform = espressif32
    board = esp32dev
    framework = arduino
    monitor_speed = 115200
    monitor_port = /dev/ttyUSB0
    lib_deps = 
        lvgl/lv_arduino@2.1.5
        bodmer/TFT_eSPI@^2.3.59
        greiman/SdFat@^2.0.4
        lvgl/lvgl@^7.10.0
        lvgl/lv_examples@^7.10.0

```

**Conclusion:** The whole output should look somthing like this and The above code should generate this result:
 <p align="center">
<img align="center" width="500" height="300" class="special-img-class" src="/images/lvgl_hardware_button_setup/tested_lvgl1.gif" alt ="ESP32" title ="ESP32" >
</p>

This is some other example running : 
 <p align="center">
<img align="center" width="500" height="300" class="special-img-class" src="/images/lvgl_hardware_button_setup/tested_lvgl.gif" alt ="ESP32" title ="ESP32" >
</p>

**Note:** Below is the example of wrong configuration.e.g, in the ***step 3***
```cpp
#define LV_HOR_RES_MAX          (100) or //10      
#define LV_VER_RES_MAX          (100) or //10  
```
**Reference Note**: 
1. wrong decleration problem : [issue](https://forum.lvgl.io/t/framebuffer-clipping-top-and-touchscreen-input-issues/1816)  
2. [ESP DEBUGGER](https://github.com/me-no-dev/EspExceptionDecoder/releases/tag/1.1.0) and follow this istallation [guide](https://forum.arduino.cc/index.php?topic=606917.msg4117591#msg4117591)
3. lvgl  event handler [guide](https://docs.lvgl.io/latest/en/html/overview/event.html#send-events-manually) 
   
#### Help in improving the guide 
Thanks Futuristic Labs Pvt. Ltd. for providing me the resources of the project.  
Please if you find any useful improvement feel free to contact at : [@pranav083](https://twitter.com/pranav083) or comment below.

