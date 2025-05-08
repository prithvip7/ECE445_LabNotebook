# Lab Notebook

**Week of 2/24:**

Proposal Review Feedback: 
  - Validation process not well thought out
      -> need to analyze how to ACTUALLY calculate high level functionality
  - Power Subsystem:
      -> 12V-5V drop may draw too much heat and burn parts. Use buck converter to limit power consumption
For breadboard demo...
  - Get imu module working, including calculating threshold for tension

Takeaway from TA meeting:
1. Focus on PCB design without IMU
2. Order Arduino wire and draft any parts we need
3. Look into buck converter

Parts bought:
  - Adafruit 0.56" 4-Digit 7-Segment Display with I2C Backpack
  - MPU-6050 3 Axis Accelerometer Gyroscope Module
  - Mini Vibration Motors Flat Coin Button-Type 10mmx3.4mm
  - Breadboard Jumper Wires Cable Kit for Arduino Projects and Raspberry Pi
  - USB-A Male to USB-B Male 2.0 Cable

PCB DESIGN:

  Block Diagram
  <img width="451" alt="image" src="https://github.com/user-attachments/assets/30644945-0c7b-4d95-bc99-e7448e7c461a" />

  Power Subsystem
  
    - Buck converter MPM3550EGLE for 12V->5V
    - Linear Regulator LM1117MP-3.3 for 5V->3V
    <img width="357" alt="image" src="https://github.com/user-attachments/assets/0a7364a5-12a7-4cb6-a5d4-7e2786afe726" />

  Feedback Subsystem
  
    - Button Motor (2x1 Connector) NEEDS PWM signal
    - 7 Segment Display (4x1 Connector) use I2C protocol
    <img width="324" alt="image" src="https://github.com/user-attachments/assets/df6d7125-cecf-4790-8dd0-24f8822c5918" />

  Sensing Subsytem
  
    - IMU (8x1 Connector) interfaced with I2C protocol
    - Reset Button (2x1 Connector)
    - Potentiometer Dial (3x1 Connector)
    <img width="430" alt="image" src="https://github.com/user-attachments/assets/1e33dc76-da35-4e59-9e96-2ba09adb5886" />

  Microcontroller
  
    - ATMega328P
    <img width="224" alt="image" src="https://github.com/user-attachments/assets/2cbd2a73-bbfc-4917-9bfe-722e291d5540" />
    
Machine Shop Meeting:
- Get box/case for watch
- Watch will be somewhat big
- Get designs and submit them to machine shop asap
  
**Week of 03/03**

TA Meeting Notes:
  - Use smd parts wherever possble
  - Use linear regulator for 5V-3.3V drop
  - Make atmega smd, dont use dip
  - Order more MPU6050, SPI display instead of I2C to not overload addresses
  - Breadboard demo coming up
  - 
Things to get done for breadboard demo:
1. Show rep counting logic using imu module
2. Show tut timer
3. Maybe reset button

Implementation for Breadboard demo...

General idea for rep counting:
1. Take the current acceleration in terms of x,y,z axis and maybe adjust for noise
2. Project the acceleration onto gravity as a reference point
3. Keep track of motion direction at beginning, top most point, return to beginnning
4. Once you return back to the beginning the rep has ended
5. Only increment counter if TUT met
   
<img width="617" alt="image" src="https://github.com/user-attachments/assets/2496b1e9-44e2-4291-8b2a-77ff3b915a1a" />

General idea for TUT tracking:
1. Keep track of time at beginning of rep
2. At 5 seconds buzz motor
3. Restart timer at bottom of rep

Parts Bought:
- 3-pack of MPU6050 3 Axis Accelerometer Gyroscope Module
- MAX7219 0.56″ 3 or 4-Digit 7-Segment Display Board

**Week of 3/10, 3/17**

... Nothing

**Week of 3/24**
!!Need to start testing ASAP!!

Problems with 1st round pcb:
- Baking Buck converter didnt work -> switching to linear regulator bc buck converter very expensive and schematic complex
- Bad arduino, programming did not work
- Bad atmega chips, causing programming to not work
  
Microcontroller Programming circuit:

<img width="484" alt="image" src="https://github.com/user-attachments/assets/aac5c050-c675-4946-a236-f68b78044af1" />

From: https://docs.arduino.cc/built-in-examples/arduino-isp/ArduinoToBreadboard/

Testing Outcome:
- Got atmega to program using arduino uno: basic code to alternate blinking 2 leds

For 3rd Round PCB:
- Create imu breakout board design
- MPU6050 reached end of life... switching to MPU6500 for breakout board
- Basing MPU Breakout board design off of MPU6500 datasheet: https://invensense.tdk.com/wp-content/uploads/2020/06/PS-MPU-6500A-01-v1.3.pdf
- Use 8x1 conn to connect breakoutboard to main board
  IMU design:
<img width="621" alt="image" src="https://github.com/user-attachments/assets/e83b007f-da3d-41ec-a5d7-c8a1198cd7e1" />

**Week of 3/31-End**
New PCB Changes
1. Changed power subsystem to now have 2 linear regulators
2. Dropped input voltage battery to 9V to avoid too much heat dissipation

First Attempt at PCB Order
   <img width="323" alt="image" src="https://github.com/user-attachments/assets/10dcabf3-8133-4b9b-acc3-b751df25b0e1" />
   
   !!!Issue with power subsystem. Failed to create coircuit properly. Need to refollow instructions on LM1117 datasheet for classic operating circuit

   --->Fixed Circuit<---
   <img width="197" alt="image" src="https://github.com/user-attachments/assets/5d3a2bfd-8a83-4c69-99da-8d0e9b84502e" />


Final Code changes:
1.On start, recalibrate the device
<img width="482" alt="image" src="https://github.com/user-attachments/assets/81e4d2ae-c2cd-44da-92b9-f8347b0fa786" />

2. Created mode toggle switch to change between forward backwards and up down motions
3. Show doNe on display and vibrate motor when meeting TUT
4. Updated TUT setting and shows value on display
5. Added code for reset
6. Keeps track of Time Under Tension and ensures the user completes rep AFTER meeting TUT or else it is not counted

<img width="308" alt="image" src="https://github.com/user-attachments/assets/5522c34e-c964-4536-900c-c46f6049344e" />      
