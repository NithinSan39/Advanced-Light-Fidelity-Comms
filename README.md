# STM32 Optical PWM Communication Link

A custom optical wireless communication system between two STM32 microcontrollers using millisecond-level PWM pulses over an LED-LDR air gap.

## Overview

This project implements one-way optical communication using:

- STM32 transmitter
- STM32 receiver
- LED as optical transmitter
- LM393 LDR sensor module as optical receiver
- UART terminal input/output

The transmitter receives characters from a PC terminal using UART interrupt reception, stores them in a circular buffer, and flashes them through an LED using a custom PWM protocol.

The receiver uses TIM2 Input Capture to measure the optical pulse widths from the LM393 LDR module and reconstructs the transmitted characters.

## Hardware

### Transmitter

- PA8: GPIO output to LED
- USART1: PC terminal input

### Receiver

- PA0: LM393 LDR digital output
- TIM2 CH1/CH2: Input Capture
- USART1: PC terminal output

## Optical PWM Protocol

| Symbol | LED ON Time | LED OFF Gap |
|---|---:|---:|
| SYNC | 200 ms | 50 ms |
| Bit 1 | 100 ms | 50 ms |
| Bit 0 | 50 ms | 50 ms |

Data is transmitted MSB first.

## Important Note

The LM393 LDR module is active-low:

- No light: output HIGH
- Light detected: output LOW

Therefore, the receiver starts timing on a falling edge and stops timing on a rising edge.

## Project Structure

```text
stm32-optical-pwm-link/
├── transmitter/
├── receiver/
├── README.md
└── .gitignore

## UART Settings

```text
Baud rate: 9600
Data bits: 8
Parity: None
Stop bits: 1
Flow control: None
```

## Status

Working prototype tested with STM32CubeIDE and HAL library.


## Hardware Setup

![Hardware setup](docs/images/Hardware Connections.jpg)