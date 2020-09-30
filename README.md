# Embedded Programming - Lab 1

Laboratory for Embedded Programming classes - Part 1 -  Based on the NodeMCU ESP8266

## About this Lab

The goal of this lab is to get started with embedded programming. We will be using the NodeMCU ESP8266 with the Arduino IDE:
- [Buy a NodeMCU ESP8266](https://www.aliexpress.com/item/32665100123.html)
- [Download the lastest Arduino IDE](https://www.arduino.cc/en/main/software)

## Steps

It should take about 2h30 min to complete all the steps

- Install Developer Environment (~20 min)
- Blink (10 min)
- General-purpose input/output (GPIO) (~15 min)
- Analog to Digital Converter (ADC) (~15 min)
- Pulse Width Modulation (PWM) (~15 min)
- Interrupts (~15 min)
- HTTP Client (~20 min)
- HTTP Server (~20 min)
- Flash ESPEasy Firmware (~20 min)

## Install Developer Environment

NodeMCU is Lua based firmware of ESP8266. Generally, ESPlorer IDE is used to write Lua scripts for the NodeMCU board. It requires to get familiar with ESPlorer IDE and Lua scripting language. However, there is another way of developing NodeMCU programs with a well-known IDE: **Arduino IDE**. 

This makes things easy for Arduino developers than learning new language and IDE for NodeMCU.

Let’s see about setting up Arduino IDE with NodeMCU.

### Arduino IDE 
Download the lastest Arduino version: [https://www.arduino.cc/en/main/software](https://www.arduino.cc/en/main/software)

![arduino-installer](assets/arduino-installer.png)

Open Arduino IDE and go to preferences:

![arduino-preferences](assets/arduino-preferences.png)

Add the following link in the Additional Boards Manager URLs: `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

![add-board-url](assets/add-board-url.png)

Close Preference window and go to Tools -> Board -> Boards Manager...

![boards-manager](assets/boards-manager.png)

In the Boards Manager window, type `esp` in the search box, esp8266 will be listed there below. 
Select latest version of board and click on install.

![install-esp-board](assets/install-esp-board.png)

After installation of the board is complete, open Tools->Board->and select NodeMCU 1.0 (ESP-12E Module).

![select-board](assets/select-board.png)

**Now Your Arduino IDE is ready for NodeMCU!!**

Let's test with the blink example, if it is not working, you may need to install the CH340G driver.


### CH340G driver

**MacOS:**
 
*Warning: Do not install if you have the current macOS Mojave 10.14 or later. macOS Mojave 10.14 (released in October 2018) includes a CH34x driver by Apple. If both Apple's and the OEM driver are installed, they will create conflicting non-functional serial ports.*

Otherwise, go to: [https://github.com/adrianmihalko/ch340g-ch34g-ch34x-mac-os-x-driver](https://github.com/adrianmihalko/ch340g-ch34g-ch34x-mac-os-x-driver)

**Windows:**

* Download the [Windows CH340 Driver](https://sparks.gogo.co.nz/assets/_site_/downloads/CH34x_Install_Windows_v3_4.zip)
* Unzip the file
* Run the installer which you unzipped
* In the Arduino IDE when the CH340 is connected you will see a COM Port in the Tools > Serial Port menu, the COM number for your device may vary depending on your system.

**Linux**

Drivers are almost certainly built into your Linux kernel already and it will probably just work as soon as you plug it in.  If not you can download the [Linux CH340 Driver](https://sparks.gogo.co.nz/assets/_site_/downloads/CH340_LINUX.zip) (but I’d recommend just upgrading your Linux install so that you get the  “built in” one).

## Blink on the NodeMCU board

Under the Blink/ folder, open the Blink.ino file

```
Embedded-Programming-Lab-1
│   README.md
│   LICENSE   
│
└───Blink
    │   Blink.ino 
```

Then, plug the NodeMCU board to your laptop.

Select the PORT as followed:

![select-port](assets/select-port.png)

Click on the upload button, if everything goes well, the LED_BUILTIN should switch on for one second and stop for two seconds

![upload-blink](assets/upload-blink.png)

Now change the blinking frequency to 500ms (500ms on and 500ms off).
Deploy your code again.

## General-purpose input/output (GPIO)

General-purpose input/output (GPIO) is a pin on an IC (Integrated Circuit). It can be either input pin or output pin, whose behavior can be controlled at the run time.

NodeMCU Development kit provides access to these GPIOs of ESP8266. The only thing to take care is that NodeMCU Dev kit pins are numbered differently than internal GPIO notations of ESP8266 as shown in below figure and table. For example, the D0 pin on the NodeMCU Dev kit is mapped to the internal GPIO pin 16 of ESP8266.

![NodeMCU-GPIOs](assets/NodeMCU-GPIOs.png)

Below table gives NodeMCU Dev Kit IO pins and ESP8266 internal GPIO pins mapping

| Pin Names on NodeMCU Development Kit| ESP8266 Internal GPIO Pin number|
| ------------- |:-------------:| 
|D0|GPIO16|
|D1|GPIO5|
|D2|GPIO4|
|D3|GPIO0|
|D4|GPIO2|
|D5|GPIO14|
|D6|GPIO12|
|D7|GPIO13|
|D8|GPIO15|
|D9/RX|GPIO3|
|D10/TX|GPIO1|
|D11/SD2|GPIO9|
|D12/SD3|GPIO10|

The GPIO’s shown in blue box (1, 3, 9, 10) are mostly not used for GPIO purpose on Dev Kit

ESP8266 is a system on a chip (SoC) design with components like the processor chip. The processor has around 16 GPIO lines, some of which are used internally to interface with other components of the SoC, like flash memory.

Since several lines are used internally within the ESP8266 SoC, we have about 11 GPIO pins remaining for GPIO purpose.

Now again 2 pins out of 11 are generally reserved for RX and TX in order to communicate with a host PC from which compiled object code is downloaded.

Hence finally, this leaves just 9 general purpose I/O pins i.e. D0 to D8.

As shown in above figure of NodeMCU Dev Kit. We can see RX, TX, SD2, SD3 pins are not mostly used as GPIOs since they are used for other internal process. But we can try with SD3 (D12) pin which mostly like to respond for GPIO/PWM/interrupt like functions.

Note that D0/GPIO16 pin can be only used as GPIO read/write, no special functions are supported on it.

**Example**

Create a new Arduino Sketch and save name it GPIO.ino:

```
Embedded-Programming-Lab-1
│   README.md
│   LICENSE   
│
└───Blink
|    │   Blink.ino 
└───GPIO
     │   GPIO.ino

```

Copy the following code:

```
uint8_t LED_Pin = D4;       // declare LED pin on NodeMCU Dev Kit

void setup() {
pinMode(LED_Pin, OUTPUT);   // Initialize the LED pin as an output
}

void loop() {
digitalWrite(LED_Pin, LOW); // Turn the LED on
delay(1000);                // Wait for a second
digitalWrite(LED_Pin, HIGH);// Turn the LED off
delay(1000);                // Wait for a second
}
```

Add plug an external LED to the D4 and Ground (GND) pins:

![wiring-blink](assets/wiring-blink.png)
