# Firmware contribution area

The supplied material did not include the STM32 firmware. A team member should add the original, buildable project here through a pull request.

## Expected structure

```text
firmware/
├── stm32/
│   ├── project-name.ioc
│   ├── Core/
│   ├── Drivers/
│   ├── Middlewares/        # if used
│   ├── .project
│   ├── .cproject
│   └── README.md
└── experiments/
    ├── arduino-uart-bridge/
    └── esp-websocket/
```

Generated `Debug/`, `Release/`, object, map, binary, and workspace-metadata files should not be committed.

## Documented firmware responsibilities

- Configure ADS1115 I2C communication on PB6/PB7.
- Select the ADC input multiplexer and PGA for voltage/current measurements.
- Apply divider and shunt scaling.
- Read the selected current range.
- Update the SH1107 OLED.
- Transmit CSV-style records over USART2 at 115200 baud.
- Handle ADC or communication failures safely.

## Required contribution notes

Document the exact STM32CubeIDE/CubeMX versions, OLED interface, pin map, ADS1115 data rate, PGA settings, range-detection logic, calibration constants, and a clean-build procedure.
