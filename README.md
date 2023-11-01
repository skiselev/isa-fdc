# isa-fdc
ISA Floppy Disk and Serial Controller

## Introduction

ISA Floppy Disk and Serial Controller card provides one floppy disk interface supporting up to two floppy drives, an RS-232 serial interface, and a BIOS extension ROM.

![ISA Floppy Disk and Serial Controller V0.9 Assembled Board](images/ISA_FDC-Board-0.9-800px.jpg)

## Specifications

* Floppy disk controller based on Intel 82077AA or National Semiconductor PC8477 FDC IC.
  * Supports IBM PC, AT, and PS/2 floppy types from 160 KB 5.25" single side disks to 2.88 MB 3.5" ED (Extended Density) disks.
  * Supports 2.88 MB IBM PS/2 drives (e.g. IBM FRU 64F4148 and IBM FRU 64F0204), that require +5V on pin 3. This feature is enabled using jumper JP1.
* Serial port.
  * Uses UART IC in PLCC package, supports newer UART versions such as 16C650 and 16C750 with extended FIFO.
  * Also compatible with 16550 and 16450 ICs in PLCC package.
  * UART I/O address and interrupt is selectable using a DIP switch. Supported I/O addresses: 3F8h, 2F8h, 3E8h, 2E8h. Supported interrupts: IRQ3, IRQ4, IRQ5, IRQ7.
* 8 KiB BIOS extension ROM.
  * Supports 28C64 EEPROMs and 27C64 UV erasable EPROMs.
  * The ROM is configured using a DIP switch: It can be enabled or disabled; the /WR line can be disabled; the address can be selected from 0C0000h to 0EE000h in 8 KiB increments.
* All functions above are optional (can be either installed or not)

## Hardware Documentation

### Schematic and PCB Layout

[Schematic - Version 0.9](KiCad/ISA_FDC-Schematic-0.9.pdf)

[PCB Layout - Version 0.9](KiCad/ISA_FDC-Board-0.9.pdf)

### Configuration Switches and Jumpers

#### SW1 - Serial Port (UART) Configuration

* SW1.1 - SW1.4 - Serial port IRQ selection
  * SW1.1: ON - IRQ3 (default setting for COM2 and COM4)
  * SW1.2: ON - IRQ4 (default setting for COM1 and COM3)
  * SW1.3: ON - IRQ5
  * SW1.4: ON - IRQ7

Note: Only one of SW1.1 - SW1.4 should be ON. All switches can be off if software doesn't use interrupt based I/O.

* SW1.5 - SW1.8 - Serial port base address selection
  * SW1.5: ON - 0x2E8 (COM4)
  * SW1.6: ON - 0x3E8 (COM3)
  * SW1.7: ON - 0x2F8 (COM2)
  * SW1.8: ON - 0x3F8 (COM1)

Note: One and only one of SW1.5 - SW1.8 switches should be ON.

####  SW2 - ROM Configuration

* SW2.1 - SW2.7 - ROM Configuration
  * SW2.1: ON - Enable ROM; OFF - Disable ROM
  * SW2.2: ON - Enable EEPROM write. This jumper is only relevant for EEPROM. It should be OFF if EPROM is used.
  * SW2.3 - SW2.7 - ROM address. Refer to the table below
* SW2.8 - Unused

##### ROM Addresses Configuration Table

SW2.3 | SW2.4 | SW2.5 | SW2.6 | SW2.7 | Start Address | End Address | SW2.3 | SW2.4 | SW2.5 | SW2.6 | SW2.7 | Start Address | End Address
----- | ----- | ----- | ----- | ----- | ------------- | ----------- | ----- | ----- | ----- | ----- | ----- | ------------- | -----------
ON    | ON    | ON    | ON    | ON    | 0xC0000*      | 0xC1FFF     | OFF   | ON    | ON    | ON    | ON    | 0xE0000**     | 0xE1FFF
ON    | ON    | ON    | ON    | OFF   | 0xC2000*      | 0xC3FFF     | OFF   | ON    | ON    | ON    | OFF   | 0xE2000**     | 0xE3FFF
ON    | ON    | ON    | OFF   | ON    | 0xC4000*      | 0xC5FFF     | OFF   | ON    | ON    | OFF   | ON    | 0xE4000**     | 0xE5FFF
ON    | ON    | ON    | OFF   | OFF   | 0xC6000*      | 0xC7FFF     | OFF   | ON    | ON    | OFF   | OFF   | 0xE6000**     | 0xE7FFF
ON    | ON    | OFF   | ON    | ON    | 0xC8000       | 0xC9FFF     | OFF   | ON    | OFF   | ON    | ON    | 0xE8000**     | 0xE9FFF
ON    | ON    | OFF   | ON    | OFF   | 0xCA000       | 0xCBFFF     | OFF   | ON    | OFF   | ON    | OFF   | 0xEA000**     | 0xEBFFF
ON    | ON    | OFF   | OFF   | ON    | 0xCC000       | 0xCDFFF     | OFF   | ON    | OFF   | OFF   | ON    | 0xEC000**     | 0xEDFFF
ON    | ON    | OFF   | OFF   | OFF   | 0xCE000       | 0xCFFFF     | OFF   | ON    | OFF   | OFF   | OFF   | 0xEE000**     | 0xEFFFF
ON    | OFF   | ON    | ON    | ON    | 0xD0000       | 0xD1FFF     | OFF   | OFF   | ON    | ON    | ON    | 0xF0000***    | 0xF1FFF
ON    | OFF   | ON    | ON    | OFF   | 0xD2000       | 0xD3FFF     | OFF   | OFF   | ON    | ON    | OFF   | 0xF2000***    | 0xF3FFF
ON    | OFF   | ON    | OFF   | ON    | 0xD4000       | 0xD5FFF     | OFF   | OFF   | ON    | OFF   | ON    | 0xF4000***    | 0xF5FFF
ON    | OFF   | ON    | OFF   | OFF   | 0xD6000       | 0xD7FFF     | OFF   | OFF   | ON    | OFF   | OFF   | 0xF6000***    | 0xF7FFF
ON    | OFF   | OFF   | ON    | ON    | 0xD8000       | 0xD9FFF     | OFF   | OFF   | OFF   | ON    | ON    | 0xF8000***    | 0xF9FFF
ON    | OFF   | OFF   | ON    | OFF   | 0xDA000       | 0xDBFFF     | OFF   | OFF   | OFF   | ON    | OFF   | 0xFA000***    | 0xFBFFF
ON    | OFF   | OFF   | OFF   | ON    | 0xDC000       | 0xDDFFF     | OFF   | OFF   | OFF   | OFF   | ON    | 0xFC000***    | 0xFDFFF
ON    | OFF   | OFF   | OFF   | OFF   | 0xDE000       | 0xDFFFF     | OFF   | OFF   | OFF   | OFF   | OFF   | 0xFE000***    | 0xFFFFF

Notes:

* - Address range conflicts with EGA / VGA BIOS.
** - 0xE0000 - 0xEFFFF address range is not available on IBM AT - Reserved for on-board BIOS extension ROMs
*** - 0xF0000 - 0xFFFFF address range is not available on IBM PC, IBM XT, and IBM AT - Used system BIOS or reserved for on-board BIOS extension ROMs

When setting ROM address make sure it doesn't conflict with system BIOS and with BIOS extension ROMs of other cards installed in the system.

#### JP1 - Connect +5V to pin 3 of floppy interface

This jumper is only required by some IBM PS/2 floppy drives that lack a separate power connector and use pin 3 for +5V. DO NOT install it in any other case as it will cause a short circuit.

### Bill of Materials

Component Type     | Reference | Description                                               | Quantity | Possible sources and notes
------------------ | --------- | --------------------------------------------------------- | -------- | --------------------------
PCB                |           | ISA FDC and Serial PCB                                    | 1        | Refer to the [RetroBrew Computers Board Inventory page](https://retrobrewcomputers.org/doku.php?id=boardinventory) for ordering information, or order from a PCB manufacturer of your choice using provided Gerber or KiCad files
Integrated Circuit | U1        | Intel N82077AA-1 or National Semiconductor PC8477BV-1 FDC | 1        | eBay. National Semiconductor PC8477BV-1 (recommended); Intel N82077AA (recommended, no tape support); Intel N82077AA-1 (tape support, FM broken); Intel N82077AA-5 (doesn't support 1 Mbps rate / ED disks); National Semiconductor PC8477AV-1 (older version of PC8277BV-1)
Integrated Circuit | U2        | 16550 or compatible UART in PLCC-44 package               | 1        | 16550/16C550 (16-byte FIFO, AT standard): Mouser 771-SC16C550BIA44-T, 701-ST16C550CJ44-F, 701-ST16C550IJ44-F, 595-TL16C550CFNR, 595-TL16C550CIFN, 595-TL16C550CFN, 926-PC16550DV; 16C650 (32-byte FIFO): Mouser 701-ST16C650ACJ44-F; 16C750 (64-byte FIFO): Mouser 771-SC16C750BIA44518, 595-TL16C750FN; 16C450 (no FIFO, not recommended): Mouser 595-TL16C450FN, 595-TL16C450FNG4, 701-ST16C450CJ44-F
Integrated Circuit | U3        | GD75232, SN75185, or SN75C185 RS-232 Drivers/Receivers    | 1        | GD75232: Mouser 595-GD75232N; SN75185: Mouser 595-SN75185N, 595-SN75185NE4; SN75C185 (recommended): Mouser 595-SN75C185NE4, 595-SN75C185N
Integrated Circuit | U4        | 28C64 EEPROM or 27C64 UV erasable EPROM                   | 1        | Mouser 556-AT28C64B15PU; Note: Atmel AT28C64B is the recommended part
Integrated Circuit | U5        | 74LS688 magnitude comparator                              | 1        | Mouser 595-CD74HCT688E, 771-74HCT688N, 595-SN74LS688N, 595-SN74LS688NE4
Integrated Circuit | U6, U7    | 74LS138 1-of-8 decoder                                    | 2        | Mouser 595-SN74AHCT138NE4, 595-SN74AHCT138N, 771-74HCT138N, 595-SN74HCT138N, 595-CD74HCT138E, 863-MC74HCT138ANG, 512-MM74HCT138N, 595-SN74ALS138AN, 595-SN74ALS138ANE4, 595-SN74LS138N
Crystal Oscillator | U8        | 1.8432 MHz full DIP oscillator                            | 1        | Mouser 520-TCF184-X. Note: This oscillator is only required for the serial port function
Capacitor          | C1 - C10  | 0.1 uF ceramic, 5.08 mm pitch                             | 10       | Mouser 810-FK28X7R1H104K, 80-C323C104K5R
Capacitor          | C11 - C14 | 10 uF, 16V, multilayer ceramic or tantalum, 5.08 mm pitch | 4        | Mouser 810-FK24X5R1C106K
Capacitor          | C15, C16  | 22 pF ceramic, 5.08 mm pitch                              | 2        | Mouser 581-SR215A220KARTR1
Capacitor          | C17       | 4.7 nF ceramic, 5.08 mm pitch                             | 1        | Mouser 581-SR215C472KAA. Note: This capacitor is only required for Intel 82077AA FDC (U1)
Resistor Array     | RR1       | 10k, 10 pin SIP, 9 resistors                              | 1        | Mouser 266-10K-RC. Note: This resistor array is only required for BIOS extension ROM function
Resistor Array     | RR2       | 1k, 6 pin SIP, 5 resistors                                | 1        | Mouser 264-1.0K-RC. Note: This resistor array is only required for FDC function
Switch             | SW1, SW2  | 16 pin DIP switch                                         | 2        | Mouser 571-54356405. Note: SW1 is required for the serial port function; SW2 is required for BIOS extension ROM functon
Connector          | P1        | 17x2 pin header                                           | 1        | Mouser 737-BHR-34-VUA. Note: This connector is only required for FDC function
Connector          | P2        | DE9M, PCB mount, right angle                              | 1        | Mouser 806-K22X-E9P-NJ15-99. Note: This connector is only required for the serial port function
Connector          | JP1       | 2 pin header                                              | 1        | Mouser 649-68002-102HLF. Note: This connector is only required for IBM PS/2 floppy disk drives that get +5V on pin 3 of floppy disk connector. DO NOT INSTALL it in any other case
IC Socket          | U1        | 68 pin PLCC through hole socket                           | 1        | Mouser 517-8468-11B1-RK-TP, 737-PLCC-68-AT. Note: This socket is only required for FDC function
IC Socket          | U2        | 44 pin PLCC through hole socket                           | 1        | Mouser 517-8444-11B1-RK-TP, 737-PLCC-44-AT. Note: This socket is only required for the serial port function
IC Socket          | U3, U5    | 20 pin 300 mil (narrow) DIP socket                        | 2        | Mouser 517-4820-3000-CP. Note: U3 socket is only required for the serial port function; U5 socket is only required for BIOS extension ROM function
IC Socket          | U4        | 28 pin 600 mil (wide) DIP socket                          | 1        | Mouser 517-4828-6000-CP. Note: U4 socket is only required for BIOS extension ROM function
IC Socket          | U6, U7    | 16 pin 300 mil DIP socket                                 | 2        | Mouser 517-4816-3000-CP. Note: U6 and U7 sockets are required for the serial port and FDC functions
IC Socket          | U8        | 4 pin 300 mil full size oscillator socket                 | 1        | Mouser 535-1107741. Note: U8 socket is only required for the serial port function
Crystal            | X1        | 24 MHz, 12 pF crystal                                     | 1        | Mouser 774-ATS240C. Note: X1 is only required for FDC function
Bracket            |           | ISA card bracket - Keystone 9200-1 (with DE9 cut out) for cards with serial port function, Keystone 9202 (with ears mounting), for cards without serial port function | 1 | Mouser 534-9200-1 - For cards with serial port function / with P2 connector; Mouser 534-9202 - For cards without serial port function / without P2 connector
Screw              |           |  Screw, 4-40 thread, 1/4" length                          | 2        | Mouser 534-9900

#### Component Selection and Substitution

* FDC IC (U1) - Either National Semiconductor PC8477 or Intel 82077AA FDC ICs can be used. These ICs come in several versions:
  * National Semiconductor PC8477BV-1 - is the later version of PC8477 FDC and it is the recommended IC
  * National Semiconductor PC8477AV-1 - earlier version of PC8477 (a couple of subtle differences are listed in PC8477B datasheet)
  * Intel N82077AA - original version of 82077AA
  * Intel N82077AA-1 - later version of 82077AA it includes tape support, but FM support is broken (Note: FM is not used by any IBM PC disk formats, and only required for some older disk formats)
  * Intel N82077AA-5 - Similar to Intel N82077AA-1 but lacks 1 Mbps transfer rate support, which is required for ED (extended density, 3.5" / 2.88 MB) disks.
* UART IC (U2) - Any 16450 / 16550 compatible UART in PLCC package can be used.
  * 16C550 UART is recommended.
  * UARTs with extended FIFO such as 16C650 and 16C750 can be used. Note that software support is required to take advantage of the extended FIFO. If software lacks such support, these UARTs will function in 16550/16450 compatible mode.
  * 16450/16C450 lacks FIFO and not recommended.
* ROM (U4) - Either EEPROM, or UV erasable EPROM can be used.
  * Atmel AT28C64B is recommended part (and it likely will be supported by built-in BIOS configuration utility)
  * Other 28C64 parts can be used, but they don't provide software controlled write protection
  * 27C64 UV erasable EPROM can be used. In this case BIOS needs to be configured prior to programming the EPROM.
* 74-series logic ICs (U5, U6, U7). Any TTL-LS or TTL compatible CMOS ICs can be used.
  * TTL compatible CMOS 74AHCT/74HCT recommended for lower power consumption and better performance.
  * Advanced TTL such as 74ALS and 74F are preferred TTL families.
  * 74LS will function as well.
* RS232 drivers/receivers IC (U3) - GD75232 or compatible chips should be used
  * SN75C185 recommended for lower power consumption
* Crystal Oscillator U8 - TTL or TTL compatible 5V CMOS in full can package.
  * If an oscillator with enable / tri state function is used it might be needed to connect enable pin (pin 1) to VCC. Please consult with oscillator's datasheet.
  * A half can oscillator can be used, in this case connect pin 11 to pin 14 with a piece of wire
* Power filtering capacitors (C11 - C14)
  * Multilayer ceramic, tantalum, or electrolytic capacitors can be used. Please observe polarity when installing tantalum and polarized electrolytic capacitors.

### Construction Notes

#### Function Configuration

The ISA Floppy Disk and Serial Controller card implements three independent functions: Floppy Disk Controller (FDC), Serial Port Controller (UART), and BIOS extension ROM (ROM). A builder can choose to build the card with any combination of these functions. For example if the card will be used in an Xi 8088 or an AT compatible system that has extended floppy support, the BIOS extension ROM is not required.

#### Shared Components

* Only power filtering capacitor C11 and the PCB is shared by all functions.
* U6 and U7 ICs are shared by FDC and UART functions, and so are their bypass capacitors - C6 and C7.
* ISA bracket type depends on presence of P2 (serial) connector:
  * If Serial Port is assembled (and P2 is present), the Keystone 9200-1 bracket should be used. This bracket has a cut out for DE9 connector and mounted on the P2.
  * If Serial Port is not assembled, the Keystone 9202 bracket should be used. This bracket has two mounting ears and it is mounted on the PCB.

#### Floppy Disk Controller Components
* U1, X1, C1, C12, C15, C16, C17, C7, RR2, P1, JP1
  * C17 can be omitted if National Semiconductor PC8477 FDC IC is used.
  * JP1 is only required for IBM PS/2 drives that don't have a separate power connector and use pin 3 of the floppy interface connector for power. It is recommended NOT to install this header if card is to be used with standard floppy drives (installing a jumper at this header with regular floppy drives will cause short circuit of +5V to GND).

#### Serial Port Components

* U2, U3, U8, C2, C3, C8, C9, C10, C13, C14, P2, SW1

#### BIOS extension ROM Components

* U4, U5, C4, C5, RR1, SW2

## Changes

### Version 0.9

* Prototype version
* Fully functional

## Errata

### All versions

* IBM AT BIOS can throw error 601 during POST if the original IBM Fixed Disk and Diskette Drive Adapter is not installed or replaced with another floppy disk controller. The system will boot and work normally after pressing F1

### Version 0.9

* Connector P1, pin 1 diameter needs to be increased
* Description for SW1 on the silk screen needs to be updated. SW1.1-SW1.4 description needs to be switched with SW1.5-SW1.8 description.
* Update board name to "ISA Floppy Disk and Serial Controller"
* Make ground traces wider (where ground plane connects to the ISA connector).
