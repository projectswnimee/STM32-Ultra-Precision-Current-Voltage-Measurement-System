# Project history

## Week 1: voltage path and local display

The first voltage divider used four 330 Ohm resistors and loaded a 12 V DUT by approximately 9.1 mA. A second 40 kOhm implementation reduced the load to approximately 0.3 mA. The final documented version used four 22 kOhm resistors, reducing the calculated load to approximately 136 microamps.

The SH1107 OLED was brought up during this stage. LM358 and MCP6002 current-sense amplifier experiments were also attempted, but the breadboard implementation produced offset, noise, and instability at low signal levels.

## Week 2: shunt ranges and communications experiments

The current front end was redesigned around passive shunts and manual range selection. The report describes 5 Ohm, 8 Ohm, and 10 Ohm shunts with range-dependent ADS1115 PGA settings.

Two ambitious logging paths were explored:

- An SD-card module encountered SPI, logic-level, library, filesystem, and supply-transient problems.
- An ESP8266/WebSocket path demonstrated access-point and raw-data experiments but did not produce a stable measurement interface.

The team retained the OLED and UART stream as the reliable output paths.

## Week 3: filtering, calibration, and enclosure

The report states that 0.1 microfarad ADC-input capacitors improved reading stability, and that the voltage and current ranges were calibrated against a reference meter. The assembly was moved into its enclosure and demonstrated as a completed prototype.

## PCB design and implementation pivot

A two-layer PCB was designed with a ground-plane strategy, but fabrication resources and schedule constraints prevented manufacture. The team pivoted to a Vero-board build so the core measurement system could be assembled and tested within the deadline.

## Engineering lessons

- Reduce DUT loading deliberately; it is part of the measurement error budget.
- A 100,000:1 current span is better handled with ranges than with a single analog gain stage.
- ADC code width is not the same as system accuracy or effective resolution.
- Breadboard parasitics and grounding matter strongly at microvolt-level signals.
- Keep one simple, testable data path while experimental features are being developed.
- Record failed approaches: they explain the final architecture and prevent repeated work.
