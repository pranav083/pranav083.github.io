---
title: "LVGL esp32 setup"
date: 2021-02-16T14:27:35+05:30
description : "Sample article showcasing basic Markdown syntax and formatting for HTML elements."
featured : true
tags : [
    "esp32","lvgl","arduino","platformIO","st7735s"
]
categories : [
    "Project",
]
series : ["Setup Guide"]
aliases : ["lvgl-arduino-esp32-setup"]
thumbnail : "images/lvgl_esp32_setup/logo.png"
draft: false
---

# Arduino Esp32 LVGL lv_arduino lv_examples st7735s

This setup guide is made using the following hardware and software

**Hardware Components And Supplies :**
1. [Esp32 Dev Module](https://components101.com/sites/default/files/component_pin/ESP32-Pinout.png)
2. ST7735s 1.8" TFT Display 128 x 160
3. USB Cable to upload the code
4. Jumper Cable (Male to Male)
5. BreadBoard

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

**Note:-** It is recommended to go with that version of the library as it is tested on the system and it is working properly.

#### Introduction:

The main aim of this project is to able to run the LVGL library on the st7735s 128 x 160 TFT display with using esp32. I am driving my system in the landscape mode you can go with portrait mode also but you to configure the setting properly(onward step 3).

**Earlier Tested** : I had tried to test with the Adafruit TFT library but it seems like their are lot of breaking changes in the library and it was not able to work properly. So, finally I decided to go with the generic library for the display st7735s.

### Learning 

**Step 1**:Create a new project in platformIO and Do the connection of the esp32 with the st7735s as follows:  


| Sl No. | ST7735s        | ESP32 | Description              |
| ------ | -------------- | ----- | ------------------------ |
| 1.     | LED            | 5v    | LED Source               |
| 2.     | SCK/TFT_SCLK   | 18    | Clock(SPI)               |
| 3.     | SDA/TFT_MISO   | 19    | DATA (SPI)               |
| 4.     | A0/DC /TFT_DC  | 25    | Data Command control pin |
| 5.     | RESET /TFT_RST | 4     | Reset pin                |
| 6.     | CS /TFT_CS     | 26    | Chip select control pin  |
| 7.     | GND            | GND   | Ground                   |
| 8.     | VCC            | 5v    | Display source           |

<p align="center">
<img align="center" width="500" height="300" class="special-img-class" src="/images/lvgl_esp32_setup/11-02-2021-13-20-32.png" alt ="ST7735s" title ="different st7735s display"  >  
</p>
<p align="center">
<img align="center" width="500" height="300" class="special-img-class" src="/images/lvgl_esp32_setup/11-02-2021-13-45-18.png" alt ="ESP32" title ="ESP32" >
</p>

**Step 2:** * Now do the following setup in the downloaded library as in my case i used PlatformIO so i will go to :  
`{your platformIO project name}/.pio/libdeps/esp32dev/TFT_eSPI/User_Setup.h`  
and for the Arduino IDE go to `/home/{USERNAME}/Arduino/libraries/TFT_eSPI/User_Setup.h`  
```cpp
    #define ST7735_DRIVER
    #define TFT_RGB_ORDER TFT_RGB  // Colour order Red-Green-Blue
    #define TFT_WIDTH  128
    #define TFT_HEIGHT 160
    #define ST7735_GREENTAB
    #define TFT_MISO 19
    #define TFT_MOSI 23
    #define TFT_SCLK 18
    #define TFT_CS   26  // Chip select control pin
    #define TFT_DC    25  // Data Command control pin
    #define TFT_RST   4  // Reset pin (could connect to RST pin)
    #define LOAD_GLCD   // Font 1. Original Adafruit 8 pixel font needs ~1820 bytes in FLASH
    #define LOAD_FONT2  // Font 2. Small 16 pixel high font, needs ~3534 bytes in FLASH, 96 characters
    #define LOAD_FONT4  // Font 4. Medium 26 pixel high font, needs ~5848 bytes in FLASH, 96 characters
    #define LOAD_FONT6  // Font 6. Large 48 pixel font, needs ~2666 bytes in FLASH, only characters 1234567890:-.apm
    #define LOAD_FONT7  // Font 7. 7 segment 48 pixel font, needs ~2438 bytes in FLASH, only characters 1234567890:-.
    #define LOAD_FONT8  // Font 8. Large 75 pixel font needs ~3256 bytes in FLASH, only characters 1234567890:-.
    //#define LOAD_FONT8N // Font 8. Alternative to Font 8 above, slightly narrower, so 3 digits fit a 160 pixel TFT
    #define LOAD_GFXFF  // FreeFonts. Include access to the 48 Adafruit_GFX free fonts FF1 to FF48 and custom fonts

    // Comment out the #define below to stop the SPIFFS filing system and smooth font code being loaded
    // this will save ~20kbytes of FLASH
    #define SMOOTH_FONT
    #define SPI_FREQUENCY  27000000
    #define SPI_READ_FREQUENCY  20000000
    #define SPI_TOUCH_FREQUENCY  2500000
```
So either you can comment everything their and put these lines or read the file uncomment these changes in the `User_Setup.h`. 

**Step 3:** Now go to the `{your platformIO project name}/.pio/libdeps/esp32dev/lv_arduino/lv_conf.h` and for the Arduino IDE go to `/home/{USERNAME}/Arduino/libraries/.pio/libdeps/esp32dev/lv_arduino/lv_conf.h`  and do the following changes :

```cpp
#if 1 /*Set it to "1" to enable content*/
/* this is the setting i found by doing a lot of testing 
otherwise the screen came to be blury you can play with 
your optimal value if not working fine*/
/* Maximal horizontal and vertical resolution to support by the library.*/
#define LV_HOR_RES_MAX          (50)      
#define LV_VER_RES_MAX          (50)      
#define LV_COLOR_DEPTH           16
#define LV_DPI                   114     /*[px]*/
#define LV_TICK_CUSTOM           1      
#define LV_USE_USER_DATA         1
// and do not cnange the rest of the file
```
**Step 4:** Now go to the `{your platformIO project name}/.pio/libdeps/esp32dev/lvgl/lv_conf_template.h` and for the Arduino IDE go to `/home/{USERNAME}/Arduino/libraries/.pio/libdeps/esp32dev/lvgl/lv_conf_template.h`  and do the following changes :
* First change the name of the file from `lv_conf_template.h` to `lv_conf.h`
* Now do the following changes to the file:
  ```cpp
  #if 1 /*Set it to "1" to enable content*/
  /* Maximal horizontal and vertical resolution to support by the library.*/
  #define LV_HOR_RES_MAX          (128)
  #define LV_VER_RES_MAX          (160)
  #define LV_COLOR_DEPTH           16
  #define LV_DPI                   114     /*[px]*/  
  ```
**Step 5:** Now open a new file in PlatformIO from the `src` folder like `main.cpp` or in the ArduinoIDE open up a new file and put this content.

```cpp
#include <Arduino.h>
#include <lvgl.h>
#include <TFT_eSPI.h>
#include <lv_examples.h>

TFT_eSPI tft = TFT_eSPI(); /* TFT instance */
static lv_disp_buf_t disp_buf;
static lv_color_t buf[LV_HOR_RES_MAX * 10];

#if USE_LV_LOG != 0
/* Serial debugging */
void my_print(lv_log_level_t level, const char * file, uint32_t line, const char * dsc)
{
    Serial.printf("%s@%d->%s\r\n", file, line, dsc);
    Serial.flush();
}
#endif

/* Display flushing */
void my_disp_flush(lv_disp_drv_t *disp, const lv_area_t *area, lv_color_t *color_p)
{
    uint32_t w = (area->x2 - area->x1 + 1);
    uint32_t h = (area->y2 - area->y1 + 1);
    tft.startWrite();
    tft.setAddrWindow(area->x1, area->y1, w, h);
    tft.pushColors(&color_p->full, w * h, true);
    tft.endWrite();
    lv_disp_flush_ready(disp);
}

void setup()
{
    Serial.begin(115200); /* prepare for possible serial debug */
    lv_init();

#if USE_LV_LOG != 0
    lv_log_register_print_cb(my_print); /* register print function for debugging */
#endif

    tft.begin(); /* TFT init */
    tft.setRotation(1); //for landscape

    lv_disp_buf_init(&disp_buf, buf, NULL, LV_HOR_RES_MAX * 5);

    /*Initialize the display*/
    lv_disp_drv_t disp_drv;
    lv_disp_drv_init(&disp_drv);
    disp_drv.hor_res = 160;
    disp_drv.ver_res = 128;
    disp_drv.flush_cb = my_disp_flush;
    disp_drv.buffer = &disp_buf;
    lv_disp_drv_register(&disp_drv);

		/* Try an example from the lv_examples repository
		 * https://github.com/lvgl/lv_examples*/
		lv_ex_btn_1();
}

void loop()
{
    lv_task_handler(); /* let the GUI do its work */
    delay(5);
}

```

**Step 6:** Now upload the program to your ESP32. Here is the content of   `platformio.ini` file:

```cpp
[env:esp32dev]
platform = espressif32
board = esp32dev
framework = arduino
lib_deps = 
	lvgl/lv_arduino@2.1.5
	bodmer/TFT_eSPI@^2.3.59
	greiman/SdFat@^2.0.4
	lvgl/lvgl@^7.10.0
	lvgl/lv_examples@^7.10.0
```

**Conclusion:** The whole output should look somthing like this:

![](/images/lvgl_esp32_setup/11-02-2021-16-09-33.png "correct configuration")
**Note:** Below is the example of wrong configuration.e.g, in the ***step 3***
```cpp
#define LV_HOR_RES_MAX          (100) or //10      
#define LV_VER_RES_MAX          (100) or //10  
```
![](/images/lvgl_esp32_setup/11-02-2021-16-13-58.png "Wrong configuration")
#### Help in improving the guide 
Thanks Futuristic Labs Pvt. Ltd. for providing me the resources of the project.
Please if you find any useful improvement feel free to contact at : [@pranav083](https://twitter.com/pranav083) or comment below.

