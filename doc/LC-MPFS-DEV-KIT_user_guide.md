# LC-MPFS-DEV-KIT User Guide

## Overview
The LC-MPFS-DEV-KIT consists of SiFive's U540 processor and Microchip’s PolarFire FPGA on a single board. The LC-MPFS-DEV-KIT is a reduced version of the HiFive Unleashed platform. The LC-MPFS-DEV-KIT enables users to create a Linux system running on the RISC-V core complex, with a large FPGA fabric accessible through the memory map. The PolarFire FPGA is shipped with a pre-configured bitstream which enables peripherals such as GPIO, UART, SPI, and I2C on the PolarFire FPGA fabric.

## Hardware Features
This section describes the features of the LC-MPFS-DEV-KIT hardware with the block diagram. 

The LC-MPFS-DEV-KIT consists of the following:
- SiFive Freedom U540 SoC
- 8 GB DDR4 with ECC
- Gigabit Ethernet port
- 32 MB Quad SPI flash connected to U540 SoC
- 1 GB SPI flash MT25QL01GBBB8ESF-0SIT connected to the PolarFire FPGA System controller
- MicroSD card for removable storage
- 300 kLE PolarFire FPGA in an FCG1152 package (MPF300T-1FCG1152)

![LC-MPFS-DEV-KIT Board](images/LC-MPFS-DEV-KIT.jpg)

## System Setup and Prerequisites
### Libero SoC Design Suite
Libero SoC design suite version 12.3 or later is needed to use the Libero project provided with the LC-MPFS-DEV-KIT.

Download the Libero SoC design suite v12.3 for Windows [here](https://www.microsemi.com/document-portal/doc_download/1244618-download-libero-soc-v12-3-for-windows).             
Download the Libero SoC design suite v12.3 for Linux [here](https://www.microsemi.com/document-portal/doc_download/1244619-download-libero-soc-v12-3-for-linux).

Along with the purchase of the LC-MPFS-DEV-KIT, customers are eligible for one platinum floating license for the Libero SoC Design Suite. Write to [mi-v-embeddedpartner@microchip.com](mi-v-embeddedpartner@microchip.com) with the subject “License Request <your organization name>” and include the 12-digit MAC ID of the two linux machines/PCs in your email.

### Solution Versions
The latest revisions of the Libero project and bitstream files are available on the [Microsemi website](https://www.microsemi.com/product-directory/soc-fpgas/5498-polarfire-soc-fpga#getting-started).

## Board Setup
The following instructions guide you to set up the LC-MPFS-DEV-KIT.

1. Switch off the power button on the LC-MPFS-DEV-KIT.

![Power Button](images/Power_On.PNG)

2. Set the pins in the DIP switch to select MSEL of 1011 (MSEL2 = 0).

![DIP Switch Setting](images/DIP_Switch.PNG)

3. To prepare the SD-card programmed with the bootloader and Linux images, see Building and Loading the Linux Image.

4. Insert the SD card into the SD card slot J10.
5. Connect the micro USB cable from J7 to the Host PC. The USB connector has two serial interfaces: the higher index serial port is used for the Linux serial console and the lower index serial port is used for JTAG debug.

![USB Connector](images/USB_Connector.PNG)

6. Update the PolarFire FPGA with the FPGA bitstream provided in Software Versions. See Programming the FPGA Using FlashPro for steps to program the FPGA.
7. The LC-MPFS-DEV-KIT is now configured as seen in Libero Block Diagram.
8. Ensure the push-button is switched on, connect the power supply to the board, and slide the power switch SW3 as shown in the following figure.

![Power on the Device](images/Power_On.PNG)

9. Configure the serial terminal in the Host PC for 115200 baud, 8 data bits, no stop bits, no parity, and no flow control. Push reset button (near the power button) on the LC-MPFS-DEV-KIT.
10. The Linux boot process can be observed on a serial terminal as shown in the following image.

![Linux Booting Messages on the Terminal](images/Linux_Booting.PNG)

Enter the following commands on the serial terminal.
```
mmc_spi 1 20000000 0
mmc read 0x80000000 0x1000 0x10000
```

![Serial Terminal](images/Serial_Command.PNG)

12. Now, boot linux with the the following command:
```
go 0x80000000
```
13. You should see linux boot. Enter the following login credentials.
```
Buildroot login: root

Password: microchip
```
The console should now look as shown in the following figure.

![Linux Booting Credentials](images/Booting_Credentials.PNG)

14. You should now see an LED flashing alongside PWM0_0 on the evaluation board.

![LED Flash On](images/LED.PNG)

## Programming Guide
The following sections explain the step-by-step procedure to download the FPGA bitstream onto the PolarFire FPGA. 
### Programming the FPGA using FlashPro
#### Windows Environment 
To program the PolarFire SoC device with the .job programming file (using FlashPro in Windows environment), perform the following steps. The link to the .job file is given in Software Versions. Ensure that the jumpers J13, J21, J28, and J31 are plugged in.
Note: The power supply switch must be switched off while making the jumper connections.

1. Connect the power supply cable to the J3 connector on the board.
2. Connect the FlashPro4 to a PC USB port and to the connector J24 (FP4 header) of the LC-MPFS-DEV-KIT hardware.
3. Power on the board using the SW3 slide switch.
4. On the host PC, launch the FlashPro Express software.
5. Click New or select New Job Project from FlashPro Express Job from Project menu to create a new job project, as shown in the following figure.       
![New Flash Pro Project](images/fp-new.png)
6. Enter the following in the New Job Project from FlashPro Express Job dialog box:
   - Programming job file: Click Browse, and navigate to the location where the .job file is located and select the file. The default location is `<download_folder>\mpf_ac466_eval\splash_df\Programming_Job`.
   - FlashPro Express job project location: Click Browse and navigate to the location where you want to save the project.        
![New Flash Pro Project](images/fp-prompt.png)

7. Click OK. The required programming file is selected and ready to be programmed in the
8. The FlashPro Express window appears as shown in the following Confirm that a programmer number appears in the Programmer field. If it does not, confirm the board connections and click Refresh/Rescan Programmers.           
![Scanning and Running the Programmer](images/fp-program.png)
9. Click RUN. When the device is programmed successfully, a RUN PASSED status is displayed as shown in the following figure. See Running the Demo, page 31 to run the demo.       
![Scanning and Running the Programmer](images/fp-programmed.png)

#### Linux Environment 

To program the PolarFire SoC device with the .job programming file (using FlashPro5 programmer in Linux environment), perform the following steps. The link to the .job file can be found in Software Versions.

1. Ensure that the jumpers J13, J21, J28, and J31 are plugged in.
Note: The power supply switch must be switched off while making the jumper connections.
2. Connect the power supply cable to the J3 connector on the board.
3. Connect the FlashPro5 to a PC USB port and to the connector J24 (FP4 header) of the board.
4. Power on the board using the SW3 slide switch.
5. On the host PC, launch the FlashPro Express (FP Express) software.
6. From the Project menu, choose Create Job Project from Programming Job.
7. Click Browse to load the Programming Job File and specify your FlashPro Express job project location. Click OK to continue.
8. Save the FlashPro Express job project.
9. Set the Programming Action in the dropdown menu to PROGRAM.
10. Click RUN. Detailed individual programmer and device status information appears in the Programmer List. Your programmer status (PASSED or FAILED) appears in the Programmer Status Bar.
See the [FlashPro Express User Guide](https://www.microsemi.com/document-portal/doc_download/137627-flashpro-express-user-guide-for-polarfire) for more information.

## Building and Loading the Linux Image
For instructions on how to build and load a Linux image, see the Linux build instructions in [top level readme](../README.md).

### FPGA Design in Libero
The Libero project interfaces the PolarFire FPGA with the U540 SoC through the ChipLink interface. The FPGA fabric is instantiated with the ChipLink to AXI bridge, while peripherals — GPIO, MMUART, SPI, and I2C — are connected to it. The ChipLink interface uses 125 MHz clock and the AXI interface uses 75 MHz clock.
The high-level block diagram of the Libero project implemented on the PolarFire FPGA is as shown in the following figure.

![LC-MPFS-DEV-KIT Board Block Diagram](images/LC_Block_Diagram.png)

#### Memory Map and GPIO Pinout

The IP cores on the LC-MPFS-DEV-KIT are accessible from the RISC-V U540 memory map as listed in the following table. 

| Peripheral | Start Address | End Address | Interrupt |
| --- | --- | --- | --- |
| GPIO | 0x2000103000 | 0x2000104000 | 7 |
| SPI_0 | 0x2000107000 | 0x2000108000 | 34 |
| I2C_0 | 0x2000100000 | 0x2000101000 | 35 |
| MMUART_0 | 0x2000104000 | 0x2000105000 | |
| SRAM | 0x2030000000 | 0x203FFFFFFF | |
| AXI_MS0 | 0x2030000000 | | |
| AXI_MS1 | 0x2600000000 | 0x263FFFFFFF | |

#### Memory Map
The GPIO implemented in the design is pinned out as a starting point for your custom design implementation. The details of the GPIO is listed in GPIO Pinout.

| GPIO | Function |
| --- | --- |
| 0 | LED4 |
| 1 | LED5  |
| 2 | Not connected |
| 3 | Not connected |
| 4 | SWITCH 9 |
| 5 | SWITCH 10 |
| 6 | Not connected |
| 7 | USB1 reset |

## Reference
Visit the following links for further reference reading materials.
### Recommended Reading
[RISC-V User-level ISA Specification](https://riscv.org/specifications/)     
[RISC-V Draft Privileged ISA Specification](https://riscv.org/specifications/privileged-isa/)     
[SiFive FU540-C000 User Manual](https://www.sifive.com/documentation/chips/freedom-u540-c000-manual/)     
[TU0844 Libero SoC PolarFire v2.2 Design Flow Tutorial](https://www.microsemi.com/document-portal/doc_download/1243632-tu0844-libero-soc-polarfire-v2-2-design-flow-tutorial)     
[HiFive Unleashed Getting Started Guide](https://www.microsemi.com/document-portal/doc_download/1243284-hifive-unleashed-getting-started-guide)   

### Reference
[PolarFire FPGA Documentation](https://www.microsemi.com/product-directory/fpgas/3854-polarfire-fpgas#documentation)     
[Libero SoC PolarFire Documentation](https://www.microsemi.com/product-directory/design-resources/3863-libero-soc-polarfire#documents)     
[FlashPro User Guide for PolarFire](https://www.microsemi.com/document-portal/doc_download/137626-flashpro-user-guide-for-polarfire)     
[FlashPro Express User Guide for PolarFire](https://www.microsemi.com/document-portal/doc_download/137627-flashpro-express-user-guide-for-polarfire)     
[PolarFire SoC Information](https://www.microsemi.com/product-directory/soc-fpgas/5498-polarfire-soc-fpga)         
[Schematics of LC-MPFS-DEV-KIT](https://www.microsemi.com/document-portal/doc_download/1244485-lc-mpfs-dev-kit-schematics) 

## Technical Support
For technical queries, visit the [Microsemi SoC Customer Portal](https://soc.microsemi.com/Portal/Default.aspx), select “PolarFire SoC” under Product Family, “MPFSXXXX” under Device Family and type in the query. Microchip’s technical support team will create a ticket, address the query and track it to completion