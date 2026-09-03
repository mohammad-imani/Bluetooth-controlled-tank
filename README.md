# 🎮 Bluetooth-Controlled Tank

A simple yet expandable Bluetooth-controlled tank project built with Arduino Nano. This project demonstrates how to control a robot tank via a serial Bluetooth connection from any smartphone.


---

##  Overview

This project started as a simple way to control a tank using Bluetooth. However, it's designed to be **modular and expandable**. Future possibilities include:
-  Adding a camera for FPV (First-Person View)
-  Implementing navigation systems using Machine Learning
-  Adding obstacle avoidance sensors

---

##  How It Works

The system uses a **Bluetooth Serial** connection between your smartphone and the Arduino Nano. Commands are sent via a serial terminal or a dedicated Bluetooth app (available on both Android and iOS).

###  Why This Approach?
-  **Cost-effective**: No complex controllers needed
-  **Flexible**: The source code can be easily customized for different needs
-  **Accessible**: Works with any smartphone that supports Bluetooth

---

##  Solutions

Throughout this project, I encountered several problems (mostly simple ones!). For each step,
You'll find detailed explanations in the corresponding section files.

---

## List of Materials
The complete list is in the BOM file.<br>
in each step we need new parts. details all are in BOM.<br>
https://github.com/mohammad1386imani-hue/Bluetooth-controlled-tank/blob/main/BOM

##  Step-by-Step Guide

###  Step 1: Establishing the Bluetooth Connection

Before anything else, we need to establish a reliable connection between the microcontroller and the mobile device. This is the **most critical part** of the system.
https://github.com/mohammad1386imani-hue/Bluetooth-controlled-tank/blob/main/first%20step

### Step 2:
Now, let's take the next step and use the serial connection established previously to start and control a motor using the L298N drive.
From this point on, we move into the control phase; we begin by implementing control using standard code, and then transition to object-oriented programming to enable the use of multiple motors.<br>
https://github.com/mohammad1386imani-hue/Bluetooth-controlled-tank/blob/main/Second-Step.md

### Step 3:
in this section, we will transport our code into OOP. because it makes the code cleaner and modular.

### Step 4:
writing the control logic.

### Step 5:
making cleaner and write the control part as a function.

### Step 6:
Solving the problems we encounter in reality
