# Telegram Communication - Sick LMS111
Relevant info for telegram communication with Sick LMS 111

This telegram listing is based on the following firmware statuses (or newer):
-  LMS1xx: V1.80 (V1.21 for LMS12x/13x)

The devices generally support automatic IP address discovery. Default IP address is:
- LMSxxx:192.168.0.1 (This can be changed using the [SOPAS Engineering Tool](https://www.sick.com/br/en/sopas-engineering-tool/p/p367244))

- IP ports:
    - 2111: CoLa A (fixed) (for LMS4000 fixed CoLa B)
    - 2112: CoLa A (can be switched to CoLa B)
    - 2213: UDP

CoLA (Command Language) has option A (ASCII), option B (Binary). You can also communicate via 



## What is CoLA (by CHATGPT)
CoLa (Command Language) is a protocol developed by SICK AG, a manufacturer of sensor systems, for use with its devices. CoLa provides a standardized way for devices to communicate with each other, allowing users to easily configure and control SICK devices from a host computer.

CoLa is a high-level protocol that is built on top of a low-level transport layer, such as TCP/IP or serial communication. It uses a text-based command language to send and receive data, making it easy to understand and implement.

CoLa supports a wide range of commands, including those for configuration, diagnostic information, and measurement data. The protocol also includes features such as error handling, authentication, and encryption, ensuring that communication between devices is secure and reliable.

CoLa is supported by a variety of SICK sensors and devices, including laser scanners, vision sensors, and 2D/3D cameras. It has become a widely-used standard in the industry, and is often used in applications such as robotics, automation, and logistics.