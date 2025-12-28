# Wiring (Câblage) Guide

## Pin mapping (ESP32)
| Function            | ESP32 Pin | Notes                          |
|---------------------|-----------|--------------------------------|
| Ultrasonic #1 TRIG  | 5         | Reservoir sensor               |
| Ultrasonic #1 ECHO  | 18        | Reservoir sensor               |
| Ultrasonic #2 TRIG  | 27        | Bowl sensor                    |
| Ultrasonic #2 ECHO  | 14        | Bowl sensor                    |
| Servo signal        | 4         | Use external 5V for power      |
| LED (status)        | 2         | Onboard LED on many ESP32 devs |
| GND (common)        | GND       | Common ground for all devices  |
| 5V (servo + sensors)| 5V        | Stable 5V supply recommended   |

## Power notes
- Servo should use a dedicated 5V supply capable of the stall current; tie grounds together with ESP32.
- HC-SR04 typically runs from 5V; if you need 3.3V logic-safe ECHO, use a voltage divider to ESP32 ECHO pins (recommended: two resistors, e.g., 10k+15k).
- Keep wiring short; avoid noise near the servo power leads.

## ASCII overview
```
          +-----------------------+
          |        ESP32          |
          |                       |
   TRIG1 -+ 5                  4 +- Servo (signal)
   ECHO1 -+ 18                 2 +- LED
   TRIG2 -+ 27              GND +- GND (common)
   ECHO2 -+ 14               5V +- (not used for servo power if external)
          +-----------------------+
               |           |
               |           +--> Servo (Power from external 5V)
               |
         HC-SR04 #1 (Reservoir)
         Vcc 5V, GND common
         TRIG -> 5, ECHO -> 18 (with level divide if needed)

         HC-SR04 #2 (Bowl)
         Vcc 5V, GND common
         TRIG -> 27, ECHO -> 14 (with level divide if needed)
```

## Quick checks
- Common ground between ESP32, sensors, servo, and 5V supply.
- If servo jitters, add a bulk capacitor (e.g., 470–1000 µF) across 5V/GND near servo.
- Secure connectors; strain-relieve wires.