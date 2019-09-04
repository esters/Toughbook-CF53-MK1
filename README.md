# Panasonic Toughbook-CF53

## Model - CF-53DAC20Q2

This is a Work-In-Progress on removing the BIOS Administrator Password by using the methods and tools available online. 
The initial work was done here - https://gist.github.com/en4rab/550880c099b5194fbbf3039e3c8ab6fd
At the moment there are following issues with this device:

* Pressing F2/F10 while on POST asks for the Administrator password 
* Devices, other than the HDD/SDD are capable of booting an OS
* Hard Disk Lock is present in the BIOS and the password is not known
* Device is pre-installed with a Windows 7 SP1 image and has a WinPE recovery partition available.

## Hardware

* Motherboard - FM111mk1MAIN PCB / DFUP1943ZA(1)PbF HF
* CPU - Intel Core i5-2520M (SR04A)
* Chipset - Intel QM67 Chipset - BD82QM67 (SLJ4M) 

## BIOS

* Vendor - American Megatrend Inc. (AMI)
* Version - V1.00L15 M91
* Platform - Aptio 4
* IC - MXIC MX25L6406E  M2I-12G

![alt text](https://github.com/esters/Toughbook-CF53/blob/master/CF53-3LTSA43202.png "Screenshot")


