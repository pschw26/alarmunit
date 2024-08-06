# Alarmunit for Griggs-type deformation apparatus 

written by Peter Schwörer (Universität Heidelberg), Philip Groß, 2023/2024

The goal of this project aims to handle conflicts arising from mechanical or electronical errors while operating a Griggs-type deformation apparatus. 
The hardware configuration comprises a Raspberry-Pi-Pico (RPP) microcontroller, along with MCP23017 board expanders. We use micropython for processing incoming and outgoing signals.

Signals can be digital or analog. The communication of RPP and board expanders works via i2c bus and up to 8 devices can get linked to the bus, thus expanding the pin capacity from 28 GPIO pins from the pico, to $28 + 8*14 = 140$ GPIO pins max. 
MCP23017 are only working for digital signals, for the analog signals there are up to three GPIO pins with ADC ability on the RPP. For further hardware information see: alarmunit/docs.

# Signal Processing & Code Structure

The signal processing is archieved via OOP-micropython programming. Therefore the Alarm.py class provides a framework for instances of the subclasses of Sensors and Actions, representing the input and output signals. 
Classes buzzer_class and mcp_pin_class serve the purpose of adapting different syntaxes. Thus buzzer_class adapts the commands for a passive buzzer to be compatible with machine functions from micropython and mcp_pin_class does
the same for the i2c communication of the MCP's with the RPP. The prupose of this is, that in this way, Alarm and Alarm.Sensor/Alarm.Action respectively, can process signals from the pins as if they were RPP pins, facilitating the Alarm class structure.

In alarmunit.py the acutal instances, thus the different sensors and the actions, which are triggered in case of error, are defined. The machine.Timer function provides a simple tool for iteration over all input signals checking for their error status
in a specified interval time.

# File/Folder Structure 

├── alarmunit/
│   ├── docs/                                   
│   │   ├── Alarmeinheit_alt.pdf                (old code for Griggs apparatus at ETH)
│   │   ├── MCP_23017_datasheet.pdf             (hardware datasheet for MCP-board expanders)
│   │   ├── raspberry_pi_pico_datasheet.pdf     (hardware datasheet for RPP)
│   │   ├── rp2-pico-20230426-v1.20.0 (1).uf2   (firmware for flashing micropython on pico)
│   ├── docs/
│   │   ├── Pico_internal_LED_test.py           (test if build in led of RPP works)
│   │   ├── i2c_scan_test.py                    (test if scan function from mcp_pin_class-class works)
│   ├── Alarm.py                                (class: defining workflow for signal processing )
│   ├── alarmunit.py                            (acutal code, containing instance definitions and frequence of iteration)
│   ├── buzzer_class.py                         (class: adapts passive buzzer syntax to micropython to provide compatibility with machine functions)
│   ├── mcp_pin_class.py                        (class: adapts i2c syntax of MCP23017 device to micropython to provide compatibility with machine functions)
