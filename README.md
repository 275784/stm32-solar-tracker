# STM32 Solar Tracker

Low-power solar tracking system based on an STM32L152 microcontroller.

The system uses two LDR sensors positioned on opposite sides of a photovoltaic panel to determine the direction of the stronger light source. The STM32 processes the sensor readings and controls an SG90 servo to adjust the panel position.

The system also uses controlled servo power switching and low-power operation to reduce unnecessary energy consumption.

## Features

- Dual LDR light sensing
- ADC-based light measurement
- Automatic solar tracking
- Servo-based panel positioning
- PWM servo control
- Hysteresis threshold to prevent unnecessary movement
- Controlled servo power switching
- Low-power Sleep mode
- Solar-powered battery charging

## System Architecture

```text
                         ┌─────────────────────┐
                         │      STM32L152      │
                         │                     │
LDR Left ───────────────►│ ADC                 │
                         │                     │
LDR Right ──────────────►│ ADC                 │
                         │                     │
                         │ Control Logic       │
                         │                     │
                         │ PWM ────────────────┼────► SG90 Servo
                         │                     │
                         │ GPIO ───────────────┼────► Servo Power
                         └─────────────────────┘


Solar Panel
     │
     ▼
Step-Down Converter
     │
     ▼
Li-Ion 2S 1A Charger
     │
     ▼
Li-Ion Battery
```

## How It Works

1. Two LDR sensors measure the light intensity on the left and right side of the panel.
2. The STM32 reads both sensors using the ADC.
3. The difference between the two readings is calculated.
4. If the difference exceeds the configured hysteresis threshold, the servo power is enabled.
5. The STM32 adjusts the servo position toward the brighter side.
6. When the measured light difference falls below the threshold, the servo remains inactive.
7. During inactive periods, the controller enters Sleep mode to reduce power consumption.

## Hardware

- STM32L152
- 2 × LDR light sensors
- SG90 servo
- Photovoltaic panel
- Step-down converter
- Li-Ion 2S 1A charger
- Li-Ion battery pack

## Technologies

- C
- Embedded C
- STM32 HAL
- STM32CubeIDE
- ADC
- PWM
- GPIO
- Timers
- Low-power Sleep mode
- Sensor-based control

## Tracking Algorithm

The controller compares the readings from the two LDR sensors:

```text
difference = LDR_Left - LDR_Right
```

The servo is moved only when:

```text
|LDR_Left - LDR_Right| > HYSTERESIS
```

If the left sensor receives more light, the panel is moved in one direction. If the right sensor receives more light, the panel is moved in the opposite direction.

The hysteresis threshold prevents the servo from continuously correcting small differences between the sensors.

## Low-Power Operation

Energy efficiency is an important part of the system design.

The STM32 periodically alternates between active operation and Sleep mode. Sensor measurements and panel positioning are performed during the active period, while the controller enters Sleep mode when no tracking adjustment is required.

The servo power supply is also controlled by the STM32 and is enabled only when panel movement is necessary.

## Implementation

The application is implemented in `Core/Src/main.c`.

STM32 peripherals used by the application include:

- ADC for LDR measurements
- TIM3 for PWM-based servo control
- GPIO for servo power control
- STM32 low-power Sleep mode

The main control loop periodically reads both LDR sensors, calculates their difference and adjusts the panel position when the difference exceeds the hysteresis threshold.

## My Contribution

I was primarily responsible for the implementation of the project, including:

- hardware assembly and integration
- STM32 firmware development
- dual-LDR sensing and ADC implementation
- servo control using PWM
- servo power control
- tracking algorithm and hysteresis
- low-power operation
- system testing and debugging

## Project Status

**Completed and tested.**

## Possible Improvements

- Add automatic LDR calibration
- Replace blocking delays with a non-blocking control loop
- Add battery voltage monitoring
- Improve energy management
- Add multi-axis tracking
- Add weather-resistant mechanical enclosure

## Authors

- Konrad Misztela