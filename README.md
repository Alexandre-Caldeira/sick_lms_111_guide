# sick_lms_111_guide
All you need to know

[Sick LMS 111](https://www.sick.com/br/en/lidar-sensors/2d-lidar-sensors/lms1xx/lms111-10100/p/p109842) is a 2D-LiDAR sensor, built by the german sensor company Sick AG. 

- # Index
    + [****Features (what can I expect)****](#features-what-can-i-expect)
    + [****Performance (how wrong does it get)****](#performance-how-wrong-does-it-get)
    + [****Mechanics and electronics (how to feed it)****](#mechanics-and-electronics-how-to-feed-it)
    + [****Interfaces (how do I connect to it)****](#interfaces-how-do-i-connect-to-it)
    + [****Capabilities (what can I do)****](#capabilities-what-can-i-do)



<!-- TODO: add dropdown, turn this into HTML github page -->
### Features (what can I expect)

- Application
	> Outdoor

- Light source	
    > Infrared (905 nm)

- Laser class	
    >1 (IEC 60825-1:2014, EN 60825-1:2014)

- Aperture angle	
    > Horizontal	2710°
    
- Scanning frequency	
    > 25 Hz, 50 Hz

- Angular resolution	
    > 0.25°, 0.5°

- Heating	
    > Yes

- Working range	
    > 0.5 m ... 20 m

- Scanning range	
    > At 10% remission	
    >    18 m
    > At 90% remission	
    >    20 m

- Amount of evaluated echoes
    > 2

- Fog correction
    > Yes

### Performance (how wrong does it get)
- Response time	
    > ≥ 20 ms

- Detectable object shape	
    > Almost any

- Systematic error	
    > ± 30 mm 1)

- Statistical error	
    > 12 mm 1)

- Integrated application	
    > Field evaluation with flexible fields

- Number of field sets	
    > 10 fields

- Simultaneous evaluation cases	
    > 10

1) Typical value; actual value depends on environmental conditions.

### Mechanics and electronics (how to feed it)
- Connection type
    > 1 x M12 round connector

- Supply voltage   
    > 10.8 V DC ... 30 V DC

- Power consumption	
    > Typ. 8 W, heating typ. 35 W

- Housing color	
    > Gray (RAL 7032)

- Enclosure rating	
    > IP67 (EN 60529, Section 14.2.7)

- Protection class	
    > III (EN 50178 (1997;10))

- Weight	
    > 1.1 kg

- Dimensions (L x W x H)	
    > 105 mm x 102 mm x 162 mm

### Interfaces (how do I connect to it)


- **Ethernet**	✔ , TCP/IP
    > Remark	
    >     OPC DA
    > Function	
    >     Host
    > Data transmission rate	
    >     10/100 MBit/s

- **Serial**	✔ , RS-232
    > Function	Host, AUX
    > Data transmission rate	
    >    9.6 kBaud ... 115.2 kBaud

- **CAN**	✔
    > Function	
    >     Extension of outputs


- Digital inputs	
    > 2 digital and 2 encoder inputs

- Digital outputs	
    > 3

- Optical indicators	
    > 7-segment display (plus 5 LEDs showing device status, contamination warning and initial condition)

![](https://cdn.sick.com/media/ZOOM/5/95/195/IM0039195.png)

https://www.sick.com/br/en/lidar-sensors/2d-lidar-sensors/lms1xx/lms111-10100/p/p109842

### Capabilities (what can I do)
Be aware that any parameterization must be followed by a log out (Run) before it can be used (LMCstartmeas). Also, the manual states:

> "After changing the scanning frequency, there will be no data telegram or answer from the devices LMS1xx, LMS5xx and TiMxxx for up to 30 seconds. The same applies when the device is powering up or rebooting."

1. Log in:                       (sMN SetAccessMode)
2. Set frequency and resolution: (sMN mLMPsetscancfg)
3. Configure scandata content:   (sWN LMDscandatacfg)
4. Configure scandata output:    (sWN LMPoutputRange)
5. Store parameters: sMN mEEwriteall
6. Set timestamp/data angle:     (sMN LSPsetdatetime)
7. Log out: sMN Run 
8. Request scan:
    - poll one telegram:         (sRN LMDscandata)
    - send data permanently:     (sEN LMDscandata)



