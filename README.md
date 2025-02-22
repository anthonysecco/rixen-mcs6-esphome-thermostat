# Rixen MCS6 ESPHome Thermostat Controller

This project implements an ESPHome-based thermostat controller by **Undermount AC** for the **Rixen MCS6 heating system**. 

The Undermount AC thermostat replaces the standard Rixen thermostat so that the Rixen MCS6 can be easily controlled through Home Assistant.  The thermostat that is found in Home Assistant reproduces the original system controls while adding two enhancements.  1) An 'auto' fan speed feature uses PID control logic to adjust the radiator fan speed to maintain the set temperature and 2) an **Enhanced Idle Mode** to reduce system short cycling.

![image](https://github.com/user-attachments/assets/790c5939-7889-453f-8d93-3fc509bb0d23)
My installation.  The undermount controller is mounted to the face of the Rixen MCS6.

## Prerequisites

Before proceeding with the installation, ensure you have the following components and tools:

1. **ESPHome Thermostat by Undermount AC** - *Required*  
   - This is the main thermostat unit being controlled by the ESP32.

2. **Sacrificial Cat5e Cable** - *Required*  
   - Used to connect the thermostat to the MCS6 controller. The length depends on where you wish to place the thermostat.

3. **18-24 AWG Wire for Fan Speed** - *Optional*  
   - Required if automatic fan speed control is desired. The length should cover the distance between the thermostat and the radiator fan.

4. **Wire Strippers** - *Required*  
   - To cut and strip wires on the cat5e cable.

5. **Home Assistant with ESPHome Installed** - *Required*  
   - For controlling, configuring, and monitoring the thermostat via Home Assistant. Follow the [ESPHome Installation Guide](https://esphome.io/guides/getting_started_command_line.html) if you haven’t already set it up.

## Installation

Follow these steps to prepare, physically install, and configure the Undermount AC Thermostat and Rixen MCS6 system.

---

### 1. **Thermostat Preparation**

Before physically installing your thermostat, it's best to program it via USB at your computer.

1. Connect the Undermount AC Thermostat to your computer using a USB cable.
2. Use the 'install firmware' link below to install the **pre-built firmware** found [here](https://github.com/anthonysecco/rixen-mcs6-esphome-thermostat/blob/main/rixen-mcs6-thermostat.yaml) directly to the ESPHome HVAC Controller via USB from a compatible browser.

   🚀[Install Firmware](https://esphome.github.io/web-installer/?config_url=https://raw.githubusercontent.com/anthonysecco/rixen-mcs6-esphome-thermostat/main/rixen-mcs6-thermostat.yaml)
   
3. Once installed, a red LED should start blinking on the device, and a wireless network named **"Rixen MCS6"** will be available.
4. The thermostat is now ready for physical installation.

---

### 2. **Physical Installation**

⚠️**Important:** Before proceeding, turn off power to the **Rixen MCS6 controller**.  Additionally, please ensure the DC power cable is unpowered when connecting to it to the thermostat.

---

#### **Step 1: Determine Installation Location**

- Select a location where you want the **temperature probe** to be positioned.  
- The probe is about 1 meter long, offering some flexibility on where the actual thermostat will be located.
- **Recommended Position:** Mount the probe 4 to 5 feet from the floor of the van, away from doors or windows to avoid inaccurate readings.  It's also advised to locate it away from the radiator fan.

---

#### **Step 2: Mount the Thermostat**

- Mount the Undermount AC thermostat using the provided screws.

---

#### **Step 3: Prepare the Cat5e Cable**

- Take the **sacrificial Cat5e cable** and ensure it is long enough to reach from the thermostat to the **Rixen MCS6 controller**, leaving extra slack for a service loop.
- Cut one end of the cable and strip the outer jacket to expose about 4 inches of the wires.  
- Separate the following wires and strip about ¼ inch of copper from each:

| **Cat5e Wire**               | **Purpose**                |
|-----------------------------|----------------------------|
| Orange                      | Constant Call              |
| Blue                        | Electric Coil              |
| White with Green Stripe     | Furnace                    |
| White with Blue Stripe      | Thermostat Call            |

- Leave the remaining four wires unused. You can cut or secure them as needed.
- Place the cable to the side for now.

---

#### **Step 4: Wire the Radiator Fan**

- Run a single wire of your choosing from the **thermostat** to the **radiator fan**.  This can be small gauge wire as it's just a signal wire.
- Locate the **blue PWM wire** on the radiator fan (sometimes tucked inside the outer jacket).
- Connect the signal wire to the blue wire at the fan
- Strip the other end of the wire (¼ inch) for connection to the thermostat.
- Place the cable to the side for now.

---

#### **Step 5: Power Wiring**

- Run a **12V or 24V DC power wire** to the thermostat. Ensure the wires are unpowered.
- Strip ¼ inch from both the positive and negative wires for proper connection.
- Place the cable to the side for now.

---

#### **Step 6: Connecting Wires to the Thermostat**

Insert the stripped wires into the correct terminals on the thermostat, and tighten the screws to secure them:

| **Terminal**                 | **Connection**                |
|-----------------------------|-------------------------------|
| **Out 1**                    | Blue       |
| **Out 2**                    | White with Green Stripe  |
| **Out 3**                    | Orange        |
| **Out 4**                    | White with Blue Stripe |
| **Out 5**                    | Radiator fan signal wire |
| **Out 6**                    | Optional recirculating pump   |
| **POS**                      | 12V or 24V Positive Wire      |
| **NEG**                      | Chassis Ground                |
| **TEMP**                     | Temperature probe             |

![image](https://github.com/user-attachments/assets/4cbbd474-cdd7-48e9-b577-9e3f433626af)

---

### 3. **Power On the Thermostat**

1. Once all wires are connected, restore power to the **DC power line** to turn on the thermostat.
2. The onboard **RGB LED** should blink red, and a wireless network named **"test"** will be available.

---

### 4. **Configure the Thermostat**

1. Connect to the **"Rixen MCS6"** network using the password: `12345678`.
2. A **captive portal** will load. Select the Wi-Fi network where your **Home Assistant** server is running and provide your credentials.
3. The thermostat will reboot and connect to your Wi-Fi network.

---

### 5. **Home Assistant Setup**

- If you don’t have ESPHome installed in Home Assistant, [follow this guide](https://esphome.io/guides/getting_started_command_line.html) to install it.
- Once the thermostat is connected to Wi-Fi, you should see it as a **newly discovered ESPHome device** under **Settings → Devices & Services** in Home Assistant.
- Click **Configure**, and the device will be added to Home Assistant.
- You should now see the **Rixen MCS6 Climate Component** in Home Assistant, which will control the **Rixen MCS6** system.

---

### 6. **Adopting the Device in ESPHome**

1. In your **ESPHome dashboard**, you should see the device with the option to **“Adopt”** (this may require restarting Home Assistant).
2. Click **Adopt**, and you’ll be able to rename the device and install the updated configuration.
3. From this point on, you have full control over the YAML configuration of the thermostat.

---

### 7. **Power On the Rixen MCS6**

If your **Rixen MCS6 controller** is powered seperately from your thermostat, go ahead and apply power.  Wait about 5 minutes or so for it to boot up and settle down.

You've completed installation and now ready to operate!

---

# Operations

Once installed and configured, the ESPHome thermostat provides the following controls and sensors in Home Assistant:

![image](https://github.com/user-attachments/assets/00ce18bd-bfc6-4bc5-a26a-35bf2be9a258)

| **Entity**                    | **Description**                                                   |
|------------------------------|-------------------------------------------------------------------|
| **Climate Card**              | Allows control of the target temperature and fan operation.      |
| **Electric Coil Switch**      | Turns the electric coil on/off when heat is called.              |
| **Furnace Switch**            | Turns the furnace (Eberspacher) on/off when heat is called.      |
| **Temperature Sensor**        | Displays the current temperature at the probe.                   |
| **Humidity Sensor**           | Displays the humidity reading at the probe.                      |
| **Radiator Fan Speed**        | Displays the radiator fan speed (0-100%).                        |

The system includes the standard Low / Medium / High / Auto fan speeds.

You will find the following diagnostic sensors to understand what is being requested from the MCS6.  These are disabled by default, but can be enabled in the Home Assistant UI.

| **Entity**                    | **Description**                                                   |
|------------------------------|-------------------------------------------------------------------|
| **Thermostat Call**              |  Shows if thermostat line is being called on the MCS6   |
| **Furnace Call**              |  Shows if furnace line is being called on the MCS6   |
| **Electric Coil Call**              |  Shows if electric coil line is being called on the MCS6   |
| **Constant Call**              |  Shows if constant line is being called on the MCS6   |

You can arrange the thermostat as you wish, I've found the below useful:

![image](https://github.com/user-attachments/assets/6787f014-b0a2-41b2-96e4-1e22866104d5)

## Auto Fan Speed

The **Auto Fan Speed** is designed to dynamically adjust the radiator fan speed to maintain a consistent and comfortable temperature in your RV. This feature helps **minimize noise** and **maximize comfort**.  If you want to understand how it works, the details are found in this section.

The auto fan speed is controlled using **PID (Proportional-Integral-Derivative)** control logic:

- **Proportional Control:** The fan speed is adjusted in proportion to the difference between the **set temperature** and the **current temperature**.
- **Integral Control:** Gradually increases the fan speed as needed to overcome the thermal load and maintain the target temperature over time.
- **Derivative Control:** Adjusts the fan speed based on how quickly the temperature error is changing. It acts as a damping factor, anticipating future error trends to reduce overshoot and oscillations, thereby stabilizing the system's response.

The system also utilizes an enhanced idle mode to extract the residual heat from the system to maintain the set temperature and reduce short cycling.

## Heat Call Cycle

The heat cycle functions similarly to a standard thermostat, with a **±0.6°C** temperature buffer to reduce cycling when the minimum heat output exceeds the RV’s thermal load.

- When the temperature drops **0.6°C** below the set temperature, the thermostat calls for heat, activating the **furnace**, **electric coil**, and **radiator fan**.
- The **radiator fan** adjusts its speed using **PID control logic**, making smooth adjustments every **30 seconds** to reach the target temperature.
- Once the **set temperature** is reached, the fan reduces to **1% speed** and continues operating until the temperature exceeds **+0.6°C above the set temperature**.
- At **+0.6°C above the set temperature**, the thermostat enters **Idle Mode**, shutting off the **furnace** and/or **electric coil**.

## Idle Modes

The system supports two idle modes depending on whether **Constant Heat Mode** is enabled.

### **Standard Idle Mode (Constant Heat Enabled)**
- If **Constant Heat Mode** is enabled (e.g., for continuous hot water production), the **fan remains off** during idle mode.

### **Enhanced Idle Mode (No Constant Heat)**
- If **Constant Heat Mode** is **not enabled**, the fan continues running to help maintain the set temperature.
- The **radiator fan behavior**:
  - Maintains a **1% speed** when the temperature is within **0.6°C of the set temperature**.
  - Increases speed if the temperature begins to drop below the **set temperature**.

The **fan will turn off** under the following conditions:
- The room temperature exceeds **+0.6°C above the set temperature**.
- The fan speed reaches **above 20%** (to prevent cold air from blowing).
- **30 minutes** have passed since idle mode started (to avoid prolonged cold air circulation).

**Note:** Fan speed and on/off states update every **30 seconds**.

## Constant Heat (Hot Water)

To request hot water, the user needs to enable their heat source (**furnace** and/or **electric coil**) and enable **Constant**.  Once the system warms up the glycol, the heat exchange block will begin to transfer heat energy.

### Optional Recirculating Pump

Output 6 on the thermostat can be used to control a relay or PWM controller for a recirculating pump, provided your hot water lines are configured for it. This feature is preconfigured but disabled by default. To enable it, simply activate it in Home Assistant.

A 60-second minimum runtime safeguard is in place by default. You can adjust this duration in the **ESPHome YAML editor**.

You can control the recirculating pump in the following ways:
- **Manual Control:** Add a button in the Home Assistant UI.
- **Automated Control:** Create an automation to activate it based on your preferences.
- **Future Enhancement:** A future update may allow the recirculating pump to activate periodically when **Constant Heat Mode** is enabled.

---
# Special Shoutout
A special thanks to Mike Goubeaux for the inspiration with his work in the Undermount AC thermostat and the MCS7 CANbus thermosat.

# License: Personal Use Only (Non-Commercial)

This project is released under a **Personal Use License**, which restricts usage to **non-commercial purposes only**. Please read the terms carefully before using or modifying the code.

## **License Terms**
Copyright © 2025 Anthony Secco

Permission is hereby granted to any person obtaining a copy of this software and associated documentation files (the "Software"), to use the Software **solely for personal, non-commercial purposes**.

### **Restrictions:**
1. The Software **may not** be used, distributed, or sold for commercial purposes without prior written permission from the copyright owner.
2. Modification of the Software for personal use is allowed, but **modified versions may not be distributed commercially**.
3. **Attribution** to the original author must be retained in any copy or modification of the Software.

## **Disclaimer**
The Software is provided **"as is"**, without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and non-infringement. In no event shall the authors be liable for any claim, damages, or other liability arising from the use of the Software.

## **Clarification of Non-Commercial Use**
**Non-commercial use** means the use of this software for personal purposes or academic projects, without any financial gain or benefit. If you wish to use this project for commercial purposes, please contact the author to request permission.
