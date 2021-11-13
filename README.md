# Panasonic Toughbook-CF53

## Model - CF-53DAC20Q2

This is a Work-In-Progress on removing the BIOS Administrator Password by using the methods and tools available online. 
The initial work was done here - https://gist.github.com/en4rab/550880c099b5194fbbf3039e3c8ab6fd.

At the moment there are following issues with this device:

* Pressing F2/F10 while on POST asks for the Administrator password 
* Devices, other than the HDD/SDD are capable of booting an OS
* Hard Disk Lock is present in the BIOS and the password is not known
* Device is pre-installed with a Windows 7 SP1 image and has a WinPE recovery partition available.

## Hardware

* Motherboard - FM111mk1MAIN PCB / DFUP1943ZA(1)PbF HF
* CPU - Intel Core i5-2520M (SR04A)
* Chipset - Intel QM67 Chipset - BD82QM67 (SLJ4M)
* RAM - 4Gb Samsung M471B5273DH0-YH9 DDR3

## BIOS

* Vendor - American Megatrend Inc. (AMI)
* Version - V1.00L15 M91
* Platform - Aptio 4
* IC - MXIC MX25L6406E M2I-12G

![alt text](https://github.com/esters/Toughbook-CF53/blob/master/CF53-3LTSA43202.png "Screenshot")

![alt text](https://github.com/esters/Toughbook-CF53/blob/master/Motherboard%20-%201.jpg "Motherboard")

![alt text](https://github.com/esters/Toughbook-CF53/blob/master/Motherboard%20-%202.jpg "Motherboard")

---

# BIOS Removal

After obtaining information from helpful people on the en4rab gist. I've decided to delete all the BIOS password entries in the dump and flash it back.

Open your favourite Hex editor. I used HxD and replaced the following entry with zeros:

```
5B C6 B6 55 11 64 6C 4B C7 A7 22 16 7D 70 D8 DA 33 27 8E 4F E9 93 44 64 9F 25 FA B9 55 51 B0 C1 0B EB 66 90 C1 1C 1C 2E 77 16 D2 A9 2D 3D 88 D0 E3 63 3E F7 99 8A F4 1D 4F B1 AA 44 05 D8 60 6B
```

