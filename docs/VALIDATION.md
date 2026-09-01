# Validation status

## Evidence classification

This repository distinguishes three types of information:

- **Observed:** visible in the supplied prototype photographs or engineering export.
- **Reported:** stated in the semester report or presentation, but raw evidence was not supplied.
- **Pending:** requires firmware, source design files, or reproducible measurements from a contributor.

## Reported results

| Item | Reported result | Evidence status |
|---|---|---|
| Voltage range | Up to 12 V DC with 0.1 V project resolution | Reported; calibration CSV pending |
| Voltage-divider loading | Approximately 136 microamps at 12 V | Calculated from 88 kOhm total resistance |
| High-current range | Less than 1% error after calibration | Reported; raw readings pending |
| Middle-current range | Less than 2% error after calibration | Reported; range boundary and raw readings pending |
| Low-current range | Approximately plus/minus 5% at the lowest end | Reported; raw readings pending |
| Input filtering | Voltage noise reduced from plus/minus 15 mV to plus/minus 2 mV; current noise from plus/minus 5 mV to plus/minus 0.5 mV | Reported; captures pending |
| OLED output | Real-time voltage/current/range display | Supported by prototype photo; firmware pending |
| UART output | 115200-baud CSV-style stream | Reported; serial capture pending |

## Required calibration dataset

Add one CSV per range under `tests/calibration/` with at least these columns:

```text
timestamp,range,reference_voltage_v,measured_voltage_v,reference_current_a,measured_current_a,adc_raw,temperature_c,supply_voltage_v,notes
```

For each range, capture:

1. Zero-input offset after warm-up.
2. At least five points across the intended range.
3. Repeated readings at every point.
4. Upward and downward sweeps to expose hysteresis or range-switch contact effects.
5. Supply voltage and ambient temperature.
6. Reference-instrument model and calibration status.

## Acceptance calculations

Use the same definitions throughout the project:

```text
absolute_error = measured - reference
percent_error = 100 x absolute_error / reference
```

Do not use percent error at a zero reference. Report zero offset separately.

## Reproducibility checklist

- [ ] STM32CubeIDE project builds from a clean clone.
- [ ] `.ioc` and tool versions are documented.
- [ ] ADC configuration and PGA selection are traceable by range.
- [ ] OLED interface and pins match the physical build.
- [ ] UART output is captured and parsed by a committed script.
- [ ] Calibration data supports every accuracy claim.
- [ ] Native KiCad files pass electrical and design-rule checks.
- [ ] BOM identifies resistor tolerance and temperature coefficient.
- [ ] Battery protection and power-rail measurements are documented.
