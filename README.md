## NFC library for Arduino Nano 33 BLE

This is an Arduino library for PN532 to use NFC technology. It's based on 
[Adafruit_NFCShield_I2C](http://goo.gl/pk3FdB)
, improved by [Seeed Studio](http://goo.gl/zh1iQh), added HSU(High Speed Uart) driver by [Elechouse](http://elechouse.com), functionality for Nano 33 BLE boards by [liswifi](https://twitter.com/liswifi). 

It works with:

+ [Elechouse NFC Module](http://goo.gl/i0EQgd)
+ [Seeed Studio NFC Shield](http://goo.gl/Cac2OH)
+ [Xadow NFC](http://goo.gl/qBZMt0)
+ [Adafruit PN532 NFC/RFID controller breakout board](http://goo.gl/tby9Sw)

### Features
+ Support all interfaces of PN532 (I2C, SPI, HSU )
+ Read/write Mifare Classic Card
+ Works with [Don's NDEF Library](http://goo.gl/jDjsXl)
+ Support Peer to Peer communication(exchange data with android 4.0+)
+ Support [mbed platform](http://goo.gl/kGPovZ)
+ Support Arduino Nano 33 BLE

### Getting Started
1. **Download [zip file](https://github.com/nestrd/PN532SPI_Nano33BLE/archive/refs/heads/main.zip) and 
extract the libraries(NDEF, PN532, PN532_SPI, PN532_HSU and PN532_I2C, PN532_SWHSU) into libraries directory of Arduino IDE.**
2. Follow the examples of the PN532 library

### Nano 33 BLE Pin SPI Connection
VIN <- VCC
GND <- GND
SCK <- SCK
D12 <- MISO
D11 <- MOSI
D10 <- SS
D9 <- RTSO

![Schematics](Schematics.png)

### To do
+ Fix libraries for SPI card emulation compatibility for Arduino Nano 33 BLE boards

## HSU Interface

HSU is short for High Speed Uart. HSU interface needs only 4 wires to connect PN532 with Arduino, [Sensor Shield](http://goo.gl/i0EQgd) can make it more easier. For some Arduino boards like [Leonardo][Leonardo], [DUE][DUE], [Mega][Mega] ect, there are more than one `Serial` on these boards, so we can use this additional Serial to control PN532, HSU uses 115200 baud rate .

To use the `Serial1` control PN532, refer to the code below.

	#include <PN532_HSU.h>
	#include <PN532.h>
	
	PN532_HSU pn532hsu(Serial1);
	PN532 nfc(pn532hsu);

	void setup(void)
	{
		nfc.begin();
		//...
	}

If your Arduino has only one serial interface and you want to keep it for control or debugging with the Serial Monitor, you can use the [`SoftwareSerial`][SoftwareSerial] library to control the PN532 by emulating a serial interface. Include `PN532_SWHSU.h` instead of `PN532_HSU.h`:

	#include <SoftwareSerial.h>
	#include <PN532_SWHSU.h>
	#include <PN532.h>
	
	SoftwareSerial SWSerial( 10, 11 ); // RX, TX

	PN532_SWHSU pn532swhsu( SWSerial );
	PN532 nfc( pn532swhsu );

	void setup(void)
	{
		nfc.begin();
		//...
	}

[Mega]: http://arduino.cc/en/Main/arduinoBoardMega
[DUE]: http://arduino.cc/en/Main/arduinoBoardDue
[Leonardo]: http://arduino.cc/en/Main/arduinoBoardLeonardo
[SoftwareSerial]: https://www.arduino.cc/en/Reference/softwareSerial