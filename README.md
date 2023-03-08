# rvr_circuitpython
[![GitHub release](https://img.shields.io/github/release/kreier/rvr_circuitpython.svg)](https://GitHub.com/kreier/rvr_circuitpython/releases/)
[![MIT license](https://img.shields.io/github/license/kreier/rvr_circuitpython)](https://kreier.mit-license.org/)


This is a collection of code that can be used to run the RVR using CircuitPython.

My notes learning to use the Sphero RVR API for the Drive to Position functionality are linked here: [Link](https://drive.google.com/file/d/1oYMbidrGnvpz_ruhsh2HalU-BsVFMmpa/view?usp=sharing)

## Pin connection definition Microcontroller - RVR

We use the following standard pins to communicate with the Sphero RVR with a HCSR04 Ultrasonic Sensor attached to the front:

|                    | blackpill | rp2040 | lilygo ESP32-S2 | Metro M4 <br>express | Metro M0 <br>express** | rp2040<br>2023 |
|--------------------|:---------:|:------:|:---------------:|:--------------------:|:----------------------:|:--------------:|
| UART TX            |     A2    |   GP4  |       IO1       |          D1          |           D2           |      GP4       |
| UART RX            |     A3    |   GP5  |       IO2       |          D0          |           D3           |      GP5       |
| Ultrasonic trigger |     B1    |  GP10  |       IO5       |          D11         |           D11          |      GP6       |
| Ultrasonic echo    |     B0    |  GP11  |       IO4       |          D10         |           D10          |      GP7       |

**Note: The Metro M0 express does not have enough memory to run both the sphero_rvr library and the imported adafruit_hcsr04 library

## sphero_rvr.py library ##
The annotated library has the API that I've given my students to use during class. Not everything is there at this point - a work in progress.

The .mpy file is the compiled version of the library for [Circuitpython 7.1.1](https://circuitpython.org). 

## API functions
- RVRDrive.drive(speed,heading)
- RVRDrive.stop()
- RVRDrive.setMotors(left,right)
- RVRDrive.drive_to_position_si(angle, x, y, speed)
- RVRDrive.reset_yaw()
- RVRDrive.sensor_start()
- RVRDrive.get_x()
- RVRDrive.get_y()
- RVRDrive.get_heading()
- RVRDrive.set_all_leds(red, green, blue)

## API documentation

RVRDrive.drive(speed,heading)
- inputs: speed, heading
- usage: drive the RVR at a given speed (0 - 255) at a heading (0 - 360).
- 0 is North, 90 is East, 180 is South, and 270 is West.

RVRDrive.stop()
- inputs: none
- usage: stop the RVR

RVRDrive.setMotors(left,right)
- inputs: left, right
- usage: set the power to the left and right sides of the RVR.
- This function limits the values to be between 0 and 255

RVRDrive.drive_to_position_si(angle, x, y, speed)
- inputs: angle, x, y, speed
- usage: The RVR will drive to specified x and y coordinates (measured in meters) relative to the
- location of the RVR when it is first turned on. This initial location is (0,0). The angle is the
- final heading of the RVR when it stops at the coordinates.
- Note: Slower speeds are more accurate.

RVRDrive.reset_yaw()
- inputs: none
- Set the current heading of the RVR to be zero. All commands after this one will use this new heading.
 
RVRDrive.sensor_start()
- inputs: none
- Prepares the RVR to start sending location data. This must be called before using RVRDrive.get_x(), RVRDrive.get_y(), or RVRDrive.get_heading().
 
RVRDrive.get_x()
- inputs: none
- returns the x coordinate of the RVR in meters relative to the origin (0,0). 
- NOTE: you must call RVRDrive.update_sensors() before using this!

RVRDrive.get_y()
- inputs: none
- returns the y coordinate of the RVR in meters relative to the origin (0,0).
- NOTE: you must call RVRDrive.update_sensors() before using this!

RVRDrive.get_heading()
- inputs: none
- returns the heading of the RVR as an angle between -180.0 and 180 degrees. North is 0, East is 90, West is -90.
- NOTE: you must call RVRDrive.update_sensors() before using this!

RVRDrive.set_all_leds(red, green, blue)
- inputs: none
- sets the red, green, and blue brightness, each as a number from 0-255.

## Sensor stream documentation API

Link follows ...

## Big thanks! ##
* [@kreier](https://github.com/kreier/) for doing testing with serial communication with RVRs and ultrasonic sensors for several boards
* [@rmerriam](https://github.com/rmerriam) for his original CPP code and documentation of the RVR API 
  * [Sphero RVR Wiki reference](https://bitbucket.org/rmerriam/rvr-cpp/wiki/browse/)
  * [Github repository for C++ API] (https://github.com/rmerriam/rvr-cpp-v2)[https://github.com/rmerriam/rvr-cpp-v2]
