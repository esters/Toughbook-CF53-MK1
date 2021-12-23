# Panasonic Toughbook-CF53

## Model - CF-53DAC20Q2

This is a Work-In-Progress on removing the BIOS Administrator Password by using the methods and tools available online. 
The initial work was done here - https://gist.github.com/en4rab/550880c099b5194fbbf3039e3c8ab6fd.

At the moment there are following issues with this device:

* Pressing F2/F10 while on POST asks for the Administrator password 
* Devices, other than the HDD/SSD are capable of booting an OS
* Hard Disk Lock is present in the BIOS and the password is not known
* Device is pre-installed with a Windows 7 SP1 image and has a WinPE recovery partition available.

Present:

- [x] BIOS image has been succesfully dumped, patched and flashed back with the BIOS password removed
- [x] HDD password has been guessed and the locked drive can be unlocked after the patched BIOS image has been flashed
- [ ] BIOS master password hashing algorithm is unknow. The password is 160 bits long (probably SHA1)

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

# BIOS Password Removal

## BIOS Password
```
5B C6 B6 55 11 64 6C 4B C7 A7 22 16 7D 70 D8 DA 33 27 8E 4F E9 93 44 64 9F 25 FA B9 55 51 B0 C1
0B EB 66 90 C1 1C 1C 2E 77 16 D2 A9 2D 3D 88 D0 E3 63 3E F7 99 8A F4 1D 4F B1 AA 44 05 D8 60 6B
```

## XOR
```
5B 93 B6 26 11 BA 6C 4D C7 E0 22 74 7D 07 D8 9A 33 2E 8E C1 E9 54 44 E8 9F 7B FA 0E 55 A2 B0 35 
0B C9 66 5C C1 EF 1C 83 77 16 D2 A9 2D 3D 88 D0 E3 63 3E F7 99 8A F4 1D 4F B1 AA 44 05 D8 60 6B
```

After xor'ing the password the output is like this:

## Output
```
00 55 00 73 00 de 00 06 00 47 00 62 00 77 00 40 00 09 00 8e 00 c7 00 8c 00 5e 00 b7 00 f3 00 f4
00 22 00 cc 00 f3 00 ad 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
```

The Output is 40 bytes long and possibly is a SHA1 hash and thus it is not a plain-text password.

After obtaining information from helpful people on the en4rab gist. I've decided to delete all the BIOS password entries in the dump and flash it back.

Open your favourite Hex editor. I used HxD and replaced the following entry with zeros:

```
5B C6 B6 55 11 64 6C 4B C7 A7 22 16 7D 70 D8 DA 33 27 8E 4F E9 93 44 64 9F 25 FA B9 55 51 B0 C1 0B EB 66 90 C1 1C 1C 2E 77 16 D2 A9 2D 3D 88 D0 E3 63 3E F7 99 8A F4 1D 4F B1 AA 44 05 D8 60 6B
```

![alt text](https://github.com/esters/Toughbook-CF53-MK1/blob/master/HxD%20-%201.png "HxD")

After replacing all the entries with the specified string above with zeros the new BIOS image was flashed to the laptop. 

Reference guide - https://davedippenaar.wordpress.com/2020/04/02/aptio_bios_passwords/

# Patched BIOS flashing

The BIOS was flashed with the AMI Flash Utility with the following options:

Setup:
```
 => Main BIOS Image
 => NVRAM
```
Then press "Flash" and it should overwrite the BIOS image with the patched image.

# Hard Disk Password Removal

If the BIOS image has been flashed with the patched version of the BIOS the hard disk will still be locked with the BIOS password as it is stored on the drive itself.

Since there was a hint that it is possible to remove the disk password if the bios password was known I followed some tutorials that I found on various forums.

For example - http://www.hddoracle.com/viewtopic.php?f=117&t=1072

Turns out in this case the hard disk password is 64 bits long. Since the BIOS password is 128 bits long, I decided to split the first 64 bits and the second 64 bits.

To unlock the drive I used a Linux Live-CD with hdparm. You should use hdparm with version 9.46 or greater which allows to send hex strings as the password. I issued the following command and the drive was unlocked without data loss:

```
# hdparm --security-unlock hex:5BC6B65511646C4BC7A722167D70D8DA33278E4FE99344649F25FAB95551B0C1 /dev/sda
security_password: 5b c6 b6 55 11 64 6c 4b c7 a7 22 16 7d 70 d8 da 33 27 8e 4f e9 93 44 64 9f 25 fa b9 55 51 b0 c1
```

To permanently disable the hard disk password execute the following command:

```
# hdparm --user-master u --security-disable hex:5BC6B65511646C4BC7A722167D70D8DA33278E4FE99344649F25FAB95551B0C1 /dev/sda
```
