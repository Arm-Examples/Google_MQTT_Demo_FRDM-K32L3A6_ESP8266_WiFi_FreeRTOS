Google MQTT Demo
================

This demo application connects to **Google Cloud IoT** through MQTT and publishes messages.

It requires a [*registered device*](https://github.com/MDK-Packs/Documentation/blob/master/Google_Cloud/README.md).

The following describes the various components and the configuration settings.

Once the application is configured you can:
- Build the application.
- Connect the debugger.
- Run the application and view messages in a debug printf or terminal window.

Google IoT Client
-----------------
The file `demo.c` configures the connection to Google IoT with these settings:
- `PROJECT_ID`: Project ID
- `CLOUD_REGION`: Cloud region
- `REGISTRY_ID`: Registry ID
- `DEVICE_ID`: Device ID

*Note*: These settings need to be configured by the user!

The file `pkey.h` configures the device private key.

*Note*: The device private key needs to be provided by the user!

RTOS: FreeRTOS Real-Time Operating System
-----------------------------------------
The real-time operating system [FreeRTOS](https://github.com/ARM-software/CMSIS-FreeRTOS) implements the resource management.

It is configured with the following settings:

- Minimal stack size \[words]: 768
- Total heap size \[bytes]: 24000
- Preemption interrupt priority: 128
- Event Recorder configuration
  - Initialize Event Recorder: 1

Refer to [Configure CMSIS-FreeRTOS](https://arm-software.github.io/CMSIS-FreeRTOS/General/html/cre_freertos_proj.html#cmsis_freertos_config) for a detailed description of all configuration options.

Socket: WiFi IoT Socket
-----------------------
This implementation uses an [IoT socket](https://mdk-packs.github.io/IoT_Socket/html/index.html) layer that connects to a 
[CMSIS-Driver WiFi](https://arm-software.github.io/CMSIS_5/Driver/html/index.html).

The file `socket_startup.c` configures the WiFi connection with these settings:
 - SSID:          network identifier
 - PASSWORD:      network password
 - SECURITY_TYPE: network security

Note: These settings need to be configured by the user!

Module: ESP8266 WiFi shield
---------------------------

The WiFi shield based on [ESP8266 SoC](https://www.espressif.com/en/products/socs/esp8266) is connected via an Arduino
header using a USART interface. It exposes a
[CMSIS-Driver WiFi](https://arm-software.github.io/CMSIS_5/Driver/html/group__wifi__interface__gr.html).

This module was tested with the ESP8266 AT command set firmware revision **1.6.2**. Refer to 
[Espressif Product Support Download](https://www.espressif.com/en/support/download/all) for more information.

Board: NXP FRDM-K32L3A6
-----------------------

The tables below list the device configuration for this board. The board layer for the NXP FRDM-K32L3A6 is using the software component `::Board Support: SDK Project Template: project_template (Variant: frdmk32l3a6)` from `NXP.FRDM-K32L3A6_BSP.13.0.0` pack.

The heap/stack setup and the CMSIS-Driver assignment is in configuration files of related software components.

The example project can be re-configured to work on custom hardware. Refer to ["Configuring Example Projects with MCUXpresso Config Tools"](https://github.com/MDK-Packs/Documentation/tree/master/Using_MCUXpresso) for information.

### System Configuration

| System Component        | Setting
|:------------------------|:-------------------------------------------------------------
| Device                  | K32L3A60VPJ1A:cm4
| Board                   | FRDM-K32L3A6
| SDK Version             | ksdk2_0
| Heap                    | 64 kB (configured in linker script K32L3A60xxx_cm4*.scf file)
| Stack (MSP)             |  1 kB (configured in linker script K32L3A60xxx_cm4*.scf file)

### Clock Configuration

| Clock                   | Setting
|:------------------------|:-------
| FIRC                    |  48 MHz
| FIRC DIV1 clock         |  48 MHz
| FIRC DIV2 clock         |  48 MHz
| FIRC DIV3 clock         |  48 MHz
| LPUART0 clock           |  48 MHz
| LPUART1 clock           |  48 MHz
| LPSPI0 clock            |  48 MHz
| LPI2C3 clock            |  48 MHz

**Note:** configured with Functional Group: `BOARD_BootClockRUN`

### GPIO Configuration and usage

| Functional Group            | Pin | Peripheral | Signal   | Identifier         | Pin Settings                           | Usage
|:----------------------------|:----|:-----------|:---------|:-------------------|:---------------------------------------|:-----------------------------------
| BOARD_InitDEBUG_UART        | N2  | LPUART0    | TX       | DEBUG_UART0_TX     | default                                | LPUART0 TX for debug console (PTC8)
| BOARD_InitDEBUG_UART        | P3  | LPUART0    | RX       | DEBUG_UART0_RX     | default                                | LPUART0 RX for debug console (PTC7)
| BOARD_InitLEDs              | D6  | GPIOA      | GPIO, 24 | RGB_RED            | default                                | User LED1 (PTA24)
| BOARD_InitLEDs              | E6  | GPIOA      | GPIO, 23 | RGB_GREEN          | default                                | User LED2 (PTA23)
| BOARD_InitLEDs              | B6  | GPIOA      | GPIO, 22 | RGB_BLUE           | default                                | User LED3 (PTA22)
| BOARD_InitButtons           | B10 | GPIOA      | GPIO,  0 | SW2                | default                                | User Button SW2 (PTA2)
| BOARD_InitButtons           | P16 | GPIOE      | GPIO,  8 | SW3                | default                                | User Button SW3 (PTE8)
| BOARD_InitButtons           | N16 | GPIOE      | GPIO,  9 | SW4                | default                                | User Button SW4 (PTE9)
| BOARD_InitButtons           | L12 | GPIOE      | GPIO, 12 | SW5                | default                                | User Button SW5 (PTE12)
| BOARD_InitARDUINO_UART      | A5  | LPUART1    | TX       | ARDUINO_LPUART1_TX | default                                | Arduino UNO R3 pin D1 (PTA26)
| BOARD_InitARDUINO_UART      | B5  | LPUART1    | RX       | ARDUINO_LPUART1_RX | default                                | Arduino UNO R3 pin D0 (PTA27)
| BOARD_InitARDUINO_SPI       | C2  | LPSPI0     | SCK      | ARDUINO_SPI_SCK    | default                                | Arduino UNO R3 pin D13 (PTB4)
| BOARD_InitARDUINO_SPI       | D2  | LPSPI0     | SOUT     | ARDUINO_SPI_MOSI   | default                                | Arduino UNO R3 pin D11 (PTB5)
| BOARD_InitARDUINO_SPI       | E2  | LPSPI0     | SIN      | ARDUINO_SPI_MISO   | default                                | Arduino UNO R3 pin D12 (PTB7)
| BOARD_InitARDUINO_SPI       | E1  | GPIOB      | GPIO,  6 | ARDUINO_SPI_SSN    | Direction Output, GPIO initial state 1 | Arduino UNO R3 pin D10 (PTB6)
| BOARD_InitPins_Arduino_PTB3 | C1  | GPIOB      | GPIO,  3 | ARDUINO_PTB3       | Direction Input                        | Arduino UNO R3 pin D9  (PTB3)

### NVIC Configuration

| NVIC Interrupt      | Priority
|:--------------------|:--------
| LPUART1             | 4
| LPSPI0              | 4

**STDIO** is routed to debug console through Virtual COM port (DAP-Link, peripheral = LPUART0, baudrate = 115200)

### CMSIS-Driver mapping

| CMSIS-Driver | Peripheral
|:-------------|:----------
| USART1       | LPUART1
| SPI0         | LPSPI0

| CMSIS-Driver VIO  | Physical board hardware
|:------------------|:------------------------------
| vioBUTTON0        | User Button SW2
| vioBUTTON1        | User Button SW3
| vioBUTTON2        | User Button SW4
| vioBUTTON3        | User Button SW5
| vioLED0           | User LED RED
| vioLED1           | User LED GREEN
| vioLED2           | User LED BLUE
|vioMotionAccelero  | MEMS accelerometer(FXOS8700CQ)
|vioMotionMagneto   | MEMS magnetometer (FXOS8700CQ)
