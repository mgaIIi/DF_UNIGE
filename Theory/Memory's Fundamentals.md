## HDD
Non-volatile storage made up of :
- **magnetic spinning platters** : the disks themselves
- **sectors**: smallest unit *that can be accessed* by a storage device 512B
- **clusters**: groups of sectors (  from 1 to 64 ) and the smallest unit of disk *allocation* for a fiile in a filesystem
- **tracks** : concentric circles made of sectors
- **cylinders** : the collection of tracks that are at the same distance from the platter edge

A *low level format* is performed on blank platters to create data structures for tracks and sectors.

*Headers and trailers* mark the beginning and the end of each sector and keep *error correcting codes*.

- **Soft errors** -> when data is recoverable
- **Hard errors** -> when a bad sector cannot be recovered and information is lost

*Logical Block addressing* **(LBA)** is the standard for physical geometry identification of disks

## SSD
Non-volatile storage made up of :
- **flash memory chips** : the data archiviation device itself
- **page** : smallest unit composed of several memory cells 2KB/16KB
- **blocks** : several pages summarized 256KB/4MB

Read and write operations can be performed at *page level* but only if the other page in the block are empty ( otherwise *write amplification* occours )

Can only delete a complete block at once.

*Modifying Data* occours with the *Program/Erase Cycle* :

1. The entire block containing targeted pages is written to memory
2. The block is marked for deletion and updated data is rewritten somewhere else
3. The actual erase occurs asynchronously ( Garbage Collection )

The **OS** uses *trim command* to notify the SSD which data pages no longer contain valid data to manage storage space more efficiently and grant longevity.

==Trim forces the Garbage collector to run and makes recovering digital 
evidence impossible==

The host cannot interact with the garbage collector and it's managed internally by the SSD's controller

## ACCESS DISK CONTENT

Available through different standard interfaces :
- **Physically** -> connect the disk with a specific connector and cable
- **Logically** -> connect and interact with the disk by using a *shared command and transport protocol* that defines how data is transferred - **ATA**, **SAS**, **NVMe**, **USB**

## ATA

#### ATA-3

Introduced HD passwords and optional security features.

Two passwords:
- **user** -> standard password to unlock the HD
- **master** -> designed to make an admin gain access if the user password was lost 

If passwords are used the disk can operate in two modes:
- **high security mode** -> the user and master password can unlock the disk
- **maximum security mode** -> the user password can unlock the disk but the
	==\ \ \ master password can unlock the disk after its content have been wiped\ \ \ ==

*Note that is a locking mechanism and no ecryption is involved*

A protected HDD will require the *SECURITY_UNLOCK* command to be executed with the correct password before any other command.
Passwords can be set through the  BIOS or specific software applications.
( *hdparm* on Linux )

#### ATA-4

Introduced  the *Host Protected Area* to provide a location not visible to the OS at the end of the disk.
It contains necessary files to install or recover the OS / perform a factory reset

ATA commands to *create the HPA* :
- *SET_MAX_ADDRESS* -> set the maximum address the user has access to
- *READ_NATIVE_MAX_ADDRESS* -> returns the maximum physical addressable sectors
- *IDENTIFY_DEVICE* -> returns only the number of sectors that a user can access

When the BIOS requires to read/write some data in the HPA it uses SET_MAX_ADDRESS with volatility bit and locking.

#### ATA-6

Introduced the *Device Configuration Overlay* that allows the apparent capabilities of an Hard disk to be limited

Created for *compatibility with newer HD* ( IDENTIFY_DEVICE is used to determine the features supported by a HD 

==\ \ \ \ DCO can be used to hide data\ \ \ \ == 

*DCO and HPA can coexist on the same HD*

The *DEVICE_CONFIGURATION_IDENTIFY* command returns the actual features and size of a disk:
- DCO is present if  *DEVICE_CONFIGURATION_IDENTIFY != IDENTIFY_DEVICE*
- An HPA might hide sectors if *READ_NATIVE_MAX_ADDRESS != DEVICE_CONFIGURATION_IDENTIFY*

==\ \ \ \ HPA on Linux can be detected using : dmesg, hdparm, disk_stat\ \ \ \ \ ==

## WINDOWS BITLOCKER

One of the most popular solutions for full disk encryption
- *symmetric enctryption* -> AES-128
- *Trusted platform module* -> used to manage cryptographic keys ( if PIN code is set up, once correctly typed it releases the keys otherwise blocks access to them )

It uses different *symmetric keys* :
- *Full Volume Encryption Key* -> to encrypt raw data - stored in encrypted drive
- *Volume Master Key* -> to encrypt the Full volume Encryption key - stored in encrypted drive
- *Key Protectors or TPM* -> to encrypt the Volume Master Key

The use of intermediate keys allows *key chaining without the need to re-encrypt* 
the raw data in case a given key protector is compromised or changed

*The Trusted Platform Module is mostly used on notebooks* and it stores a *Storage Root Key* that is used to decrypt the *Volume Master Key* only if the system passes the *Secure Boot Check*

![[./assets/Bitlocker.png]]

==\ \ \ Bitlocker does not protect from other users on the computer\ \ \ \ ==

