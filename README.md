# PWM DC Motor Speed Controller (555 Timer Based)

A simple, low-cost DC motor speed controller built around the classic NE555P timer IC. The 555 runs in astable mode with an asymmetric charge/discharge network to generate a variable duty-cycle PWM signal, which drives a TIP122 Darlington transistor to switch the motor. A single potentiometer gives smooth control over motor speed.

## Schematic

![Schematic](https://github.com/Noraiz963/DC-Motor-Controller/blob/7232ff4c8819d8c29ba488f6f875c46c61898974/Screenshot%202026-07-21%20233521.png)

## PCB Layout

![PCB Layout](https://github.com/Noraiz963/DC-Motor-Controller/blob/7232ff4c8819d8c29ba488f6f875c46c61898974/Screenshot%202026-07-21%20233532.png)

## 3D Render

![3D Render](https://github.com/Noraiz963/DC-Motor-Controller/blob/7232ff4c8819d8c29ba488f6f875c46c61898974/Screenshot%202026-07-21%20233726.png)

![3D Render](https://github.com/Noraiz963/DC-Motor-Controller/blob/7232ff4c8819d8c29ba488f6f875c46c61898974/Screenshot%202026-07-21%20233705.png)

## How It Works

The circuit is built around four functional blocks:

### 1. Power Input & Filtering
Power enters through a 3.5mm terminal (J2). A 1000µF capacitor (C1) smooths the supply rail so switching transients from the motor don't disturb the 555's timing. A 2.2k resistor (R1) drives an LED (D4) as a power-on indicator.

### 2. Timing / PWM Generation Network
The NE555P (U1) is configured as an astable oscillator. Instead of a single resistor, the charge and discharge paths are separated using two steering diodes (D1, D2) around a 100k potentiometer (RV1). This lets the charge time and discharge time be adjusted independently, producing a duty cycle that can be swept across a wide range — from mostly off to mostly on — rather than being stuck above 50% like a standard 555 astable. R2 (1k) limits current into the 555's internal discharge transistor. C2 (100nF) is the timing capacitor. C3 (100nF) bypasses the CONTROL pin per the 555 datasheet recommendation, keeping the internal reference stable.

### 3. Output / Motor Drive Stage
The 555's OUT pin drives the base of Q1 (TIP122) through a 1k base resistor (R3). The TIP122 is a Darlington transistor capable of switching several amps, acting as the actual power switch for the motor while the 555 only supplies the low-current control signal. As RV1 is adjusted, the PWM duty cycle changes, which changes the average voltage delivered to the motor — and therefore its speed.

### 4. Flyback Protection
Because a DC motor is an inductive load, switching it off abruptly causes a voltage spike as the motor's magnetic field collapses. A flyback diode (D3, 1N4007) is placed across the motor output to safely clamp this spike and protect Q1 from damage.

### Signal Flow
```
J2 (DC in) → C1 filter → 555 astable (RV1 sets duty cycle)
           → 555 OUT → R3 → Q1 (TIP122) base
           → Q1 switches motor at J1, PWM-style
           → D3 clamps inductive kickback on every switch-off
```

## Bill of Materials

| Ref | Value | Description |
|-----|-------|-------------|
| U1 | NE555P | Timer IC, astable PWM generator |
| Q1 | TIP122 | NPN Darlington transistor, motor switch |
| RV1 | 100k | Potentiometer, speed/duty-cycle control |
| R1 | 2.2k | LED current limiting resistor |
| R2 | 1k | 555 discharge current limiting resistor |
| R3 | 1k | Q1 base resistor |
| C1 | 1000µF | Input filter / bulk capacitor |
| C2 | 100nF | Timing capacitor |
| C3 | 100nF | 555 CONTROL pin bypass capacitor |
| D1, D2 | 1N4007 | Charge/discharge steering diodes |
| D3 | 1N4007 | Flyback protection diode |
| D4 | LED | Power-on indicator |
| J1 | 3.5mm terminal | Motor output |
| J2 | 3.5mm terminal | DC power input |



## Specifications

- **Board size:** A4 sheet design, compact 4-hole mount PCB
- **Input:** DC supply via 3.5mm terminal (voltage rating depends on motor and TIP122 limits)
- **Output:** Switched DC to motor via 3.5mm terminal, protected with flyback diode
- **Control:** Single potentiometer, adjustable PWM duty cycle
- **Design tool:** KiCad 10.0.4


## Usage

1. Connect a DC power supply to J2, matching your motor's voltage requirements.
2. Connect the DC motor to J1.
3. Power on — the LED (D4) will light to confirm the supply is active.
4. Turn RV1 to adjust motor speed from low to high.

## Notes / Possible Improvements

- Add a flyback/snubber network closer to the motor terminals for noisy or high-inductance motors.
- Consider a heatsink for Q1 if driving higher current motors continuously.
- A reverse-polarity protection diode on the input could be added for extra robustness.



## Author

Designed by MAKER.
