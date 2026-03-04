# Energy Theft Detection System using GSM

A microcontroller-based embedded system that detects unauthorized energy tapping on electrical distribution lines and delivers automated real-time alerts via GSM SMS. Built as a capstone project for B.E. Electrical and Electronics Engineering at CVR College of Engineering.

---

## Problem Statement

Electricity theft is a critical challenge in power distribution networks, causing significant revenue losses and contributing to power outages. Manual detection is time-consuming and labor-intensive. This project automates theft detection by continuously monitoring current differentials across distribution lines and triggering immediate alerts when unauthorized tapping is detected.

---

## System Overview

The system uses two current transformers to monitor input and output current simultaneously. When a discrepancy indicating unauthorized tapping is detected, the microcontroller triggers a relay to trip the circuit, activates a buzzer alarm, displays an alert on the LCD, and sends an SMS notification to a registered mobile number via the GSM module.

```
Power Supply --> Energy Meter --> Microcontroller (Arduino ATmega328)
                    |                     |
              Current Transformer    [LCD Display]
                    |                [GSM Module] --> SMS Alert
                 Load 1              [Buzzer Alarm]
                 Load 2              [Relay Control]
```

---

## Hardware Components

| Component | Description |
|---|---|
| Arduino Uno (ATmega328) | Main microcontroller |
| GSM SIM900 Module | Wireless SMS communication |
| Current Transformer (x2) | Input and output current sensing |
| ACS712 Current Sensor | Hall-effect based current detection |
| Energy Meter | Power consumption measurement |
| 16x2 LCD Display | Real-time status display |
| Relay Module | Automatic circuit tripping |
| Regulated Power Supply | 5V DC regulated supply |
| Buzzer | Audible theft alert |

---

## Communication Protocols Used

- **UART** — Serial communication between Arduino and GSM module via SoftwareSerial
- **GSM AT Commands** — SMS transmission using AT+CMGS command set
- **I2C** — LCD display interface
- **Analog ADC** — Current sensor signal acquisition on analog pins A0, A1, A2

---

## Key Features

- Continuous real-time monitoring of energy consumption across two load lines
- Automatic detection of current differential indicating unauthorized tapping
- Instant SMS alert to registered mobile number when theft is detected
- Automatic relay tripping to disconnect the tampered circuit
- LCD display showing units consumed, billing amount, and theft alert status
- Periodic billing SMS sent every 3 units consumed
- Mobile number registration via GSM initialization routine

---

## System Operating Modes

**Normal Mode** — Both loads operating within allowed range. Units consumed and billing amount displayed on LCD and sent via SMS periodically.

**Theft Detection Mode** — Current differential detected between input and output sensors. System trips the circuit via relay, activates buzzer, displays ENERGY THEFT ALERT on LCD, and sends immediate SMS notification to electricity board.

**Standby Mode** — No load connected. System in monitoring state, LCD displays initialization status.

---

## Software

**Language:** C++ (Arduino IDE)

**Key Libraries:**
- `LiquidCrystal.h` — LCD control
- `SoftwareSerial.h` — UART communication with GSM module
- `stdio.h` — Standard I/O functions

**Core Logic:**
```cpp
// Theft detection condition
if(statusSensor1 == LOW && statusSensor2 == LOW) {
    // Both sensors detect current anomaly
    // Trigger relay, buzzer, LCD alert, GSM SMS
    mySerial.write("ENERGY THEFT DETECTED ALERT..");
}

// Normal billing condition
if(units % 3 == 0) {
    // Send periodic billing SMS
    mySerial.write("Billing Units: X  Amount: Y");
}
```

---

## Results

**Three operating states verified:**

1. Normal load operation — units consumed displayed correctly on LCD with periodic SMS billing
2. Simultaneous theft and normal load — system detected differential and triggered alert
3. Theft-only condition — circuit tripped automatically, SMS alert delivered in under 5 seconds

Photos of working prototype, LCD display states, and SMS alert screenshots are included in the `/results` folder.

---

## Project Structure

```
energy-theft-detection-gsm/
├── src/
│   └── energy_theft_detection.ino   # Main Arduino firmware
├── hardware/
│   ├── block_diagram.png            # System block diagram
│   ├── circuit_diagram.png          # Full circuit schematic
│   └── power_supply_diagram.png     # Regulated power supply design
├── results/
│   ├── lcd_display_modes.jpg        # LCD operating state photos
│   ├── working_prototype.jpg        # Experimental setup photo
│   └── sms_alerts.jpg               # SMS notification screenshots
├── docs/
│   └── project_report.pdf           # Full technical report
└── README.md
```

---

## Future Improvements

This project was built on GSM-based communication. The natural evolution toward modern IoT architecture includes:

- **WiFi/MQTT Integration** — Replace GSM with ESP32 + MQTT broker for cloud connectivity
- **Real-Time Dashboard** — Stream sensor data to a Streamlit or Grafana dashboard for live monitoring
- **Data Logging Pipeline** — Store consumption data in a time-series database (InfluxDB or AWS DynamoDB) for historical analysis
- **Predictive Analytics** — Apply machine learning on consumption patterns to detect anomalies before theft occurs
- **Multi-Node Monitoring** — Scale to monitor multiple distribution lines from a central server
- **Mobile App Integration** — Push notifications via Firebase instead of SMS

---

## Team

| Name | Roll Number |
|---|---|
| P. Ajay Kumar | 18B81A0262 |
| B. Nithish Kumar | 18B81A0284 |
| B. Sowmya | 18B81A02A2 |
| Y. Penny Venkat | 18B81A0287 |

**Guide:** Mrs. V. Vimala Devi, Senior Assistant Professor, EEE Department  
**Institution:** CVR College of Engineering, Hyderabad  
**Year:** 2022

---

## Academic Context

This project was submitted in partial fulfillment of the requirements for the degree of Bachelor of Technology in Electrical and Electronics Engineering at CVR College of Engineering, affiliated to JNTU Hyderabad.

---

## License

This project is open source and available under the MIT License.
