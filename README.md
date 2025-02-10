# Rixen MCS6 ESPHome Thermostat Controller

This project implements an ESPHome-based thermostat controller by **Undermount AC** for the **Rixen MCS6 heating system**. 

The Undermount AC thermostat replaces the standard Rixen thermostat so that the Rixen MCS6 can be easily controlled through Home Assistant.  The thermostat that is found in Home Assistant reproduces the original system controls while adding an 'auto' fan speed feature uses PID control logic to adjust the radiator fan speed to maintain the set temperature.

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
2. Use the button provided in the project (or in your ESPHome dashboard) to install the **pre-built firmware** directly to the ESPHome HVAC Controller via USB from a compatible browser.
3. Once installed, a red LED should start blinking on the device, and a wireless network named **"test"** will be available.
4. The thermostat is now ready for physical installation.

---

### 2. **Physical Installation**

**Important:** Before proceeding, turn off power to the **Rixen MCS6 controller**.

---

#### **Step 1: Determine Installation Location**

- Select a location where you want the **temperature probe** to be positioned.  
- The probe is about 1 meter long, offering some flexibility.  
- **Recommended Position:** Mount the probe 4 to 5 feet from the floor of the van, away from doors or windows to avoid inaccurate readings.

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

---

#### **Step 4: Wire the Radiator Fan**

- Run a wire from the **thermostat** to the **radiator fan**.
- Locate the **blue PWM wire** on the radiator fan (sometimes tucked inside the outer jacket).
- Connect the signal wire to the blue wire at the fan, and strip the other end of the wire (¼ inch) for connection to the thermostat.

---

#### **Step 5: Power Wiring**

- Run a **12V or 24V DC power wire** to the thermostat. Ensure the wires are unpowered before connecting.
- Strip ¼ inch from both the positive and negative wires for proper connection.

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
| **Out 6**                    | Unused                        |
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

1. Connect to the **"test"** network using the password: `12345678`.
2. A **captive portal** will load. Select the Wi-Fi network where your **Home Assistant** server is running and provide your credentials.
3. The thermostat will reboot and connect to your Wi-Fi network.

---

### 5. **Home Assistant Setup**

- If you don’t have ESPHome installed in Home Assistant, [follow this guide](https://esphome.io/guides/getting_started_command_line.html) to install it.
- Once the thermostat is connected to Wi-Fi, you should see it as a **newly discovered ESPHome device** under **Settings → Devices & Services** in Home Assistant.
- Click **Configure**, and the device will be added to Home Assistant.
- You should now see the **Undermount AC Climate Component** in Home Assistant, which will control the **Rixen MCS6** system.

---

### 6. **Adopting the Device in ESPHome**

1. In your **ESPHome dashboard**, you should see the device with the option to **“Adopt”** (this may require restarting Home Assistant).
2. Click **Adopt**, and you’ll be able to rename the device and install the updated configuration.
3. From this point on, you have full control over the YAML configuration of the thermostat.

---

### 7. **Power On the Rixen MCS6**

1. Switch the power back on to the **Rixen MCS6 controller**.
2. Wait about 5 minutes for it to boot up.

---

## Operations

Once installed and configured, the ESPHome thermostat provides the following controls and sensors in Home Assistant:

| **Entity**                    | **Description**                                                   |
|------------------------------|-------------------------------------------------------------------|
| **Climate Card**              | Allows control of the target temperature and fan operation.      |
| **Electric Coil Switch**      | Turns the electric coil on/off when heat is called.              |
| **Furnace Switch**            | Turns the furnace (Eberspacher) on/off when heat is called.      |
| **Temperature Sensor**        | Displays the current temperature at the probe.                   |
| **Humidity Sensor**           | Displays the humidity reading at the probe.                      |
| **Radiator Fan Speed**        | Displays the radiator fan speed (0-100%).                        |

Congratulations! You have successfully set up the **Undermount AC ESPHome Thermostat Controller** for your **Rixen MCS6**. From here, you can make any further customizations using the YAML configuration in the ESPHome dashboard.

---

## Auto Fan Logic

The **auto fan speed** is designed to dynamically adjust the radiator fan speed to maintain a consistent and comfortable temperature in your RV. This feature helps **minimize noise** and **maximize comfort**.

---

### How It Works

The auto fan speed is controlled using **PID (Proportional-Integral-Derivative)** control logic:

- **Proportional Control:** The fan speed is adjusted in proportion to the difference between the **set temperature** and the **current temperature**.
- **Integral Control:** Gradually increases the fan speed as needed to overcome the thermal load and maintain the target temperature over time.

---

### Idle Mode Behavior

- Once the thermostat detects that the **set temperature** has been reached, it will move to **idle mode**, turning off the **furnace** and/or **electric coil**.
- The **radiator fan** may continue operating during idle mode to maintain your set temperature extractining the remaining heat from the system.
- The fan will turn off in the following conditions:
  - If the fan speed reaches **25% or lower**, or
  - If **10 minutes** have passed while in idle mode.

This prevents the fan from blowing cold air when no additional heat is available.

---

### Heat Call Cycle

- When the actual temperature drops to **1°F below the set temperature**, the thermostat will:
  - Call for heat by turning on the **furnace**, **electric coil**, and **radiator fan**.
  - The cycle will continue until the set temperature is reached again.

---

### Notes:
- The **auto fan logic** has only been tested using **Fahrenheit**. If you plan to use Celsius, additional testing or configuration adjustments may be required.


---
## Hot Water

<insert notes on hot water>

---
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
**Non-commercial use** means the use of this software for personal purposes or academic projects, without any financial gain or benefit. If you wish to use this project for commercial purposes, please [contact the author](mailto:youremail@example.com) to request permission.

For further information or requests, please send me a DM.
