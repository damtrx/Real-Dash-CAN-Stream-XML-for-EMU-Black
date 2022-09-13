# Real Dash USB to CAN XML example for EMU Black

This repository contains XML files and maybe other examples needed to connect Ecumaster EMU Black with USB to CAN Analyzer via CAN bus network.

This project is still work in progress, but it can be used as base configuration for Real Dash.

I'm choosing Real Dash because it is a cheap alternative to ADU units and can be installed on almost any Android device.


# Table of Contents
1. [Hardware](#Hardware)
2. [Software](#Software)
3. [XML File](#xml-File)
4. [Real Dash configuratio](#real-dash-configuration)
5. [To Do](#to-to)


## Hardware
I'm using USB to CAN module to connect the EMU Black CAN interface to my Android Headunit device which didn't support Bluetooth module.

Using USB connection is more reliable and can give me an option to send back CAN Stream data for inputs like buttons. The purpose of this might be to change Boost tables or other things during normal operation.

Custom CAN Stream data can be used to analyze engine performance like Knock Levels or EGT temperature.

I have used the following cheap USB to CAN module:

[LINK - USB to CAN Bus Converter Adapter serial port TO CAN /RS232 232 TO CAN With TVS surge protection](https://www.aliexpress.com/item/32994257402.html?spm=a2g0o.order_list.0.0.2b6b1802VYj2gS)

![USB to CAN](/img/USB-to-CAN-Analyzer.jpg)

### Wiring

Connect CAN-L and CAN-H to the coresponding pins of the USB to CAN Device.
Connect Ground to USB to CAN Device ground.

If 120ohm resistor is present in the CAN Bus network then remove the jumper from the USB to CAN Device.


## Software

I had to change a few parameters to the USB to CAN device because it didn't work properly on my EMU Black.

Software used to program the device is located here:

[USB-to-Can Software](/src/USB-CAN-Software/Program/USB-CAN(V7.20).exe)

If needed explore the [software](/src/USB-CAN-Software) folder for more information about the module

### Software configuration

![Software config](/img/usb-to-can-software.png)

   1. Select **COM** port X
   2. The default BAUD rate is **2000000**. Select **Change bps** and change it to **115200**
      Device should blink a couple of times. (three if remember correctly). After baud change is written into the device change the baud rate connection in the software.
   3. Select **CAN Speed** that matches the speed of the **EMU Black**. In my case 500kpbs (the same as the bluetooth module)
   4. Use **Normal mode**
   5. Use **Standard Frame**
   6. Select **Start and Monitor** to verify the device operation. CAN BUS state on the EMU Black software should be green. If everyting is fine there should be data comming into the USB to CAN software. If not reset the device by shortening the **Default** pin and try again. It took me a few days to figure this out.(when **default** is shorten disconnect the USB from the device and connect it again)


## XML File
XML file configration can be found here. 
It uses the Default base ID which is 0x600 but can be changed in EMU Black Software
[RD-EMUBlack.xml](./src/rd-emublack.xml)

All the data inside the XML file has comments for convenience.

## Real Dash configuration

Start Real dash for android.
   1. Add a new connection - **Adapters(CAN)**
   2. Select **CAN Analyzer**
   3. Select **Serial/USB** connection
   4. Use **115200 kbps** 
   5. Use Custom XML transformation file provided in this repo
   6. Select Frame mode - **Normal**
   7. Choose **500kbps** for speed
   8. Use **CAN Monitor** to check connectivity

## To Do

- [ ] Assign buttons to Real Dash
- [ ] FIX Check Engine Lights BITflag parameters
- [ ] Confugre custom Knock can stream