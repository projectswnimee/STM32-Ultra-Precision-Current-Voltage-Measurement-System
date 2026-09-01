# Technical overview

## Design intent

The project was designed to measure low-voltage DC systems while reducing the load placed on the device under test. It combines two analog front ends with a shared ADS1115:

- A high-value 4:1 divider for voltage measurement.
- A manually selected shunt for differential current measurement.

The STM32 reads the ADC, applies range-dependent scaling, updates the OLED, and transmits a serial record to a PC.

## Voltage channel

The final documented divider consists of four 22 kOhm resistors in series. The ADS1115 A0 input is connected across the bottom resistor.

| Quantity | Value at a 12 V input |
|---|---:|
| Total resistance | 88 kOhm |
| Divider output | 3.0 V |
| DUT loading | 136.4 microamps |
| Total divider dissipation | 1.64 mW |
| Input capacitor | 0.1 microfarad |
| Approximate RC cutoff | 18 Hz |

These are calculated values. Resistor tolerance, ADC input behavior, reference accuracy, wiring resistance, and calibration affect the realized measurement uncertainty.

## Current channels

The report describes three shunt values selected by a 3P4T rotary switch. The voltage across the active shunt is measured differentially between ADS1115 inputs A1 and A2.

| Shunt | Reported current region | Calculated shunt drop |
|---:|---:|---:|
| 5 Ohm | 20-100 mA | 100-500 mV |
| 8 Ohm | 2-20 mA | 16-160 mV |
| 10 Ohm | 1-200 microamps | 10 microvolts-2 mV |

The lowest 1 microamp point produces only 10 microvolts across the 10 Ohm shunt. That is close to the ADC code width at its narrowest documented full-scale range, so accuracy at the bottom of the range depends heavily on offset, noise, effective resolution, and calibration.

## Filtering

The report describes 0.1 microfarad capacitors from ADS1115 analog inputs to ground. The calculated single-ended RC cutoff values are:

| Path | Source resistance | Approximate cutoff |
|---|---:|---:|
| Voltage divider | 88 kOhm | 18 Hz |
| 5 Ohm shunt | 5 Ohm | 318 kHz |
| 8 Ohm shunt | 8 Ohm | 199 kHz |
| 10 Ohm shunt | 10 Ohm | 159 kHz |

The physical differential network and the ADS1115 input impedance should be reviewed when the native schematic and firmware are added.

## Documented interfaces

| Function | STM32 pins | Notes |
|---|---|---|
| ADS1115 I2C | PB6 SCL, PB7 SDA | Address documented as `0x48` |
| UART logging | PA2 TX, PA3 RX | USART2 at 115200 baud |
| OLED | PB8, PB9, PB10, PC3 in the final pin table | Report sections disagree on SPI versus I2C; verify from firmware and wiring |
| Range selection | Various GPIO / rotary switch | Exact GPIO map not supplied |

## Serial output

The report gives the intended record format as:

```text
Voltage(V),Current(mA),Range
```

No raw serial capture or logging script was included in the source package. These should be added under `tests/results/` and `tests/tools/`.

## Source inconsistencies requiring verification

| Topic | Conflicting documentation | Repository treatment |
|---|---|---|
| Voltage resolution | 0.1 V in the report; 0.001 V steps on one slide | 0.1 V retained as the documented project requirement |
| ADC sample rate | One slide claims at least 2 kS/s | Rejected: the ADS1115 maximum programmable data rate is 860 SPS |
| Middle current range | 2-20 mA in several places; 0.2-20 mA elsewhere | Marked for firmware/calibration verification |
| OLED interface | SPI pin map in the final integration table; I2C in another report section and schematic export | Marked for wiring/firmware verification |
| Buck output | 3.3 V in one section; 5 V feeding the Blackpill elsewhere | Described generically as regulated rails until verified |
| PCB status | PCB designed, but fabrication not completed | PCB export is included; Vero board identified as the working prototype |

## Primary component references

- [Texas Instruments ADS1115 product page and datasheet](https://www.ti.com/product/ADS1115)
- [STMicroelectronics STM32F411CE product page](https://www.st.com/en/microcontrollers-microprocessors/stm32f411ce.html)
