# Maixduino Selfie

Link:

[https://mensuel.framapad.org/p/maixduino\_tests-9hkw?lang=en](https://mensuel.framapad.org/p/maixduino\_tests-9hkw?lang=en)


Tutorial:

   *  [https://www.elektormagazine.com/articles/artificial-intelligence-for-beginners-1/](https://www.elektormagazine.com/articles/artificial-intelligence-for-beginners-1/)
   * [https://www.elektormagazine.com/articles/artificial-intelligence-for-beginners-2](https://www.elektormagazine.com/articles/artificial-intelligence-for-beginners-2)


OS:

   * Ubuntu
   * Mac
   * Windows (?)


Installation 

   * Codium: [https://vscodium.com/](https://vscodium.com/)
       * Available extensions: [https://open-vsx.org/](https://open-vsx.org/)
   * Platform IO: [https://platformio.org/](https://platformio.org/) (or extension for Codium),
       * [https://platformio.org/platformio-ide](https://platformio.org/platformio-ide)


Windows:

   * [https://github.com/VSCodium/vscodium/releases](https://github.com/VSCodium/vscodium/releases): example: [https://github.com/VSCodium/vscodium/releases/download/1.46.1/VSCodium-win32-x64-1.46.1.zip](https://github.com/VSCodium/vscodium/releases/download/1.46.1/VSCodium-win32-x64-1.46.1.zip)
   * Change/replace in codium\resources\app\product.json
      "extensionsGallery": {

        "serviceUrl": "[https://marketplace.visualstudio.com/\_apis/public/gallery](https://marketplace.visualstudio.com/\_apis/public/gallery)",

        "itemUrl": "[https://marketplace.visualstudio.com/items](https://marketplace.visualstudio.com/items)"

      },


Mac:

   * Tips to have Platform IO extension in VSCodium: [https://github.com/VSCodium/vscodium/blob/master/DOCS.md](https://github.com/VSCodium/vscodium/blob/master/DOCS.md)
   * vi /Applications/VSCodium.app/Contents/Resources/app/product.json
   * find and replace extensionsGallery with
   * "extensionsGallery": {
   *     "serviceUrl": "[https://marketplace.visualstudio.com/\_apis/public/gallery](https://marketplace.visualstudio.com/\_apis/public/gallery)",
   *     "itemUrl": "[https://marketplace.visualstudio.com/items](https://marketplace.visualstudio.com/items)"
   * }


Codium FAQ: [https://github.com/VSCodium/vscodium/blob/master/DOCS.md](https://github.com/VSCodium/vscodium/blob/master/DOCS.md) 



## Alternative

Installation Platform IO (Windows / Linux), global installation for all (no virtual environment): [https://docs.platformio.org/en/latest/core/index.html](https://docs.platformio.org/en/latest/core/index.html)

   * install Python 3
   * pip3 install pyserial
   * pip3 install -U platformio
   * pio
   * pio --version
   * PlatformIO, version 4.3.4


Maixduino configuration for Platform IO:

    [https://docs.platformio.org/en/latest/boards/kendryte210/sipeed-maixduino.html](https://docs.platformio.org/en/latest/boards/kendryte210/sipeed-maixduino.html)

    

## Codium / Platform IO



### New Project

   * New Project: PIO Home / Open / New Project:
       * Name: MaixduinoTest
       * Board: Sipeed MAIXDUINO
       * Framework: Arduino (see Elektor [https://www.elektormagazine.com/articles/artificial-intelligence-for-beginners-1/)](https://www.elektormagazine.com/articles/artificial-intelligence-for-beginners-1/))
       * OK (wait, take a while)


## Example

   * Selfie: [https://maixduino.sipeed.com/en/libs/sipeed\_ov2640.html](https://maixduino.sipeed.com/en/libs/sipeed\_ov2640.html)
       * [https://github.com/sipeed/Maixduino/tree/master/libraries/Sipeed\_OV2640/examples/selfie](https://github.com/sipeed/Maixduino/tree/master/libraries/Sipeed\_OV2640/examples/selfie)


File **platform.io**: 

    ; PlatformIO Project Configuration File
    ;
    ;   Build options: build flags, source filter
    ;   Upload options: custom upload port, speed and extra flags
    ;   Library options: dependencies, extra library storages
    ;   Advanced options: extra scripting
    ;
    ; Please visit documentation for the other options and examples
    ; [https://docs.platformio.org/page/projectconf.html](https://docs.platformio.org/page/projectconf.html)

    [env:sipeed-maixduino]
    platform = kendryte210
    board = sipeed-maixduino
    framework = arduino
    

File **src/main.cpp**:

    #include <Arduino.h>

    #include <Sipeed\_OV2640.h>
    #include <Sipeed\_ST7789.h>

    SPIClass spi\_(SPI0); // MUST be SPI0 for Maix series on board LCD
    Sipeed\_ST7789 lcd(320, 240, spi\_);
    Sipeed\_OV2640 camera(FRAMESIZE\_QVGA, PIXFORMAT\_RGB565);

    void setup()
    {
        Serial.begin(115200);
        lcd.begin(15000000, COLOR\_RED);
        if(!camera.begin())
          Serial.printf("camera init fail\n");
        else
          Serial.printf("camera init success\n");
        camera.run(true);
    }

    void loop()
    {
      uint8\_t*img = camera.snapshot();
      if(img == nullptr || img==0)
        Serial.printf("snap fail\n");
      else
        lcd.drawImage(0, 0, camera.width(), camera.height(), (uint16\_t*)img);
    }

Compile:

   * click check button in bottom blue bar or
   * select PIO / Project Tasks / Build
