# Real Dash USB to CAN XML example for EMU Black

This repository contains XML files and maybe other examples needed to connect EcuMaster EMU Black with USB to CAN Analyzer via CAN bus network.

This project is still work in progress, but it can be used as base configuration for Real Dash.

I'm choosing Real Dash because it is a cheap alternative to ADU units and can be installed on almost any Android device.


# Table of Contents
1. [Hardware](#Hardware)
2. [Software](#Software)
3. [XML File](#xml-File)
4. [Real Dash configuration](#real-dash-configuration)
5. [Real Dash enable CAN Switch](#real-dash-can-switch)
6. [CAN Switch feedback](#can-switch-feedback-switch)
7. [Real Dash enable custom CAN Frames](#real-dash-custom-can-frames)
8. [Real Dash example](#real-dash-screenshots)
9. [Real Dash example file](/src/newdash2.rd)
10. [To Do](#to-to)



## Hardware
I'm using USB to CAN module to connect the EMU Black CAN interface to my Android Head unit device which didn't support Bluetooth module.

Using USB connection is more reliable and can give me an option to send back CAN Stream data for inputs like buttons. The purpose of this might be to change Boost tables or other things during normal operation.

Custom CAN Stream data can be used to analyze engine performance like Knock Levels or EGT temperature.

I have used the following cheap USB to CAN module:

[LINK - USB to CAN Bus Converter Adapter serial port TO CAN /RS232 232 TO CAN With TVS surge protection](https://www.aliexpress.com/item/32994257402.html?spm=a2g0o.order_list.0.0.2b6b1802VYj2gS)

![USB to CAN](/img/USB-to-CAN-Analyzer.jpg)

### Wiring

Connect CAN-L and CAN-H to the corresponding pins of the USB to CAN Device.
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
   6. Select **Start and Monitor** to verify the device operation. CAN BUS state on the EMU Black software should be green. If everything is fine there should be data coming into the USB to CAN software. If not reset the device by shortening the **Default** pin and try again. It took me a few days to figure this out.(when **default** is shorten disconnect the USB from the device and connect it again)


## XML File
XML file configuration can be found here. 
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

   
## Real Dash CAN Switch 
   RealDash developers are awesome. I was able to send CAN Switch 1 to the EMU Black from RealDash back to the ECU
   ![CAN Switch config](/img/cansw1.png)

   XML File is updated!
   Use RealDash actions to assign value of 1 to the CAN Switch.

   CAN Switches that works with RealDash at the moment
   [] Can Switch 1
   [x] Can Switch 2
   [x] Can Switch 3
   [x] Can Switch 4
   [x] Can Switch 5
   [x] Can Switch 6
   [x] Can Switch 7
   [] Can Switch 8


## CAN Switch feedback switch
   It turned out that when you send 1 as value to the CAN network and flip a switch the EMU Black doesn't return the state.
   This is needed to allow RealDash to distinguish the state of the Switch

   Create a new 0x667 can stream with all the Switches and send the bits to RealDash
   ![Switch CAN State](/img/can-sw-feedback.png)

## Real Dash custom CAN frames 
   Here is an example of how to send custom CAN frame into the network. I'm using Knock sensor voltage value.
   Decided to multiple the value by 100 and then scale it back to the proper value from RealDash xml
   ![Custom CAN frame](/img/KnockSensorCANFrame.png)

## Real Dash screenshots
### Main screen features
  - Songs - Prev/Next
  - Lambda graph
  - OIL pressure/ OIL Temp
  - RPM
  - TPS
  - Ignition advance
  - Battery
  - Coolant
  - [RealDash Example File](./src/newdash2.rd) 
 ![Main screen](/img/Dashboard.png)

### Knock sensor page
  - Knock sensor value
  - Knock sensor table
  - TPS
![Knock screen](/img/KnockValues.png)

### PoC button 
  - CAN Switch 1
![CAN PoC](/img/ButtonExample.png)

## To Do

- [X] Assign buttons to Real Dash
- [ ] FIX Check Engine Lights BITflag parameters
- [X] Configure custom Knock can stream