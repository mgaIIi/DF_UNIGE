# Console.dd

```
Something fishy is going on. Someone gave us this image of an unpartitioned FAT filesystem containing a JPEG picture. However, this volume seems to have just been (re)formatted to hide something.

Can you reconstruct the original partition scheme and recover the content of the original partition?
```

## Verify the image

```
> sha256sum --check console.dd.sha256
console.dd: OK
```

## Analysis

I started by running **TSK - fls** to list files and directories in the image since I read that there was a JPEG picture inside the provided image

```
> fls console.dd

r/r 4:	xbox.jpg
v/v 130867:	$MBR
v/v 130868:	$FAT1
v/v 130869:	$FAT2
V/V 130870:	$OrphanFiles
```

There was indeed a JPEG file that I could analyse so I extracted it directly by its name using  **TSK - fcat** 

```
fcat xbox.jpg console.dd > xboxjpg_extracted.jpg 
```

![](./assets/xboxjpg_extracted.jpg)


After that I ran **TSK - fsstat** to display general details of the file-system provided and **TSK - mmls** to get the partition layout.


```
> fsstat console.dd

File System Type: FAT12

OEM Name: mkfs.fat
Volume ID: 0xb9e28db8
Volume Label (Boot Sector): NO NAME    
Volume Label (Root Directory):
File System Type Label: FAT12   

Sectors before file system: 0

File System Layout (in sectors)
Total Range: 0 - 8191
* Reserved: 0 - 0
** Boot Sector: 0
* FAT 0: 1 - 6
* FAT 1: 7 - 12
* Data Area: 13 - 8191
** Root Directory: 13 - 44
** Cluster Area: 45 - 8188
** Non-clustered: 8189 - 8191

METADATA INFORMATION
--------------------------------------------
Range: 2 - 130870
Root Directory: 2

CONTENT INFORMATION
--------------------------------------------
Sector Size: 512
Cluster Size: 2048
Total Cluster Range: 2 - 2037
fcat ps5.jpg linuxfs_extracted.dd > ps5jpg_extracted.jpg
FAT CONTENTS (in sectors)
--------------------------------------------
49-76 (28) -> EOF
```

```
> mmls console.dd

GUID Partition Table (EFI)
Offset Sector: 0
Units are in 512-byte sectors

      Slot           Start                     End                    Length               Description
000:  -------      0000000000      0000002047     0000002048      Unallocated
001:  002           0000002048      0000008158      0000006111       Linux filesystem
002:  Meta         0000008159      0000008190      0000000032      Partition Table
003:  -------      0000008159      0000008191       0000000033     Unallocated
004:  Meta         0000008191      0000008191       0000000001      GPT Header
```

From the output of **TSK - mmls** I saw that apparently there was a Linux filesystem located in the first partition so I tried to extract it using **TSK - mmcat** 

```
> mmcat console.dd 1 > linuxfs_exctracted.dd
```

After that I ran again **TSK - fsstat** on the newly acquired raw image in order to gather informations about that

```
> fsstat linuxfs_extracted.dd

FILE SYSTEM INFORMATION
--------------------------------------------
File System Type: NTFS
Volume Serial Number: 46ACAF237FF4C000
OEM Name: NTFS    
Version: Windows XP

METADATA INFORMATION
--------------------------------------------
First Cluster of MFT: 4
First Cluster of MFT Mirror: 381
Size of MFT Entries: 1024 bytes
Size of Index Records: 4096 bytes
Range: 0 - 65
Root Directory: 5

CONTENT INFORMATION
--------------------------------------------
Sector Size: 512
Cluster Size: 4096
Total Cluster Range: 0 - 762
Total Sector Range: 0 - 6109

$AttrDef Attribute Values:
$STANDARD_INFORMATION (16)   Size: 48-72   Flags: Resident
$ATTRIBUTE_LIST (32)   Size: No Limit   Flags: Non-resident
$FILE_NAME (48)   Size: 68-578   Flags: Resident,Index
$OBJECT_ID (64)   Size: 0-256   Flags: Resident
$SECURITY_DESCRIPTOR (80)   Size: No Limit   Flags: Non-resident
$VOLUME_NAME (96)   Size: 2-256   Flags: Resident
$VOLUME_INFORMATION (112)   Size: 12-12   Flags: Resident
$DATA (128)   Size: No Limit   Flags: 
$INDEX_ROOT (144)   Size: No Limit   Flags: Resident
$INDEX_ALLOCATION (160)   Size: No Limit   Flags: Non-resident
$BITMAP (176)   Size: No Limit   Flags: Non-resident
$REPARSE_POINT (192)   Size: 0-16384   Flags: Non-resident
$EA_INFORMATION (208)   Size: 8-8   Flags: Resident
$EA (224)   Size: 0-65536   Flags: 
$LOGGED_UTILITY_STREAM (256)   Size: 0-65536   Flags: Non-resident
```

From the output I saw that it was a NTFS filesystem, used by the Microsoft-Windows OS, unexpected since the name of that partition suggested that it would be a Linux filesystem but this could be due to the fact the GPT partition description may have been altered.
Just as I did before I ran **TSK - fls** on the new image to list files and directories

```
> fls liuxfs_extracted.dd
r/r 4-128-1:	$AttrDef
r/r 8-128-2:	$BadClus
r/r 8-128-1:	$BadClus:$Bad
r/r 6-128-1:	$Bitmap
r/r 7-128-1:	$Boot
d/d 11-144-2:	$Extend
r/r 2-128-1:	$LogFile
r/r 0-128-1:	$MFT
r/r 1-128-1:	$MFTMirr
r/r 9-128-2:	$Secure:$SDS
r/r 9-144-3:	$Secure:$SDH
r/r 9-144-4:	$Secure:$SII
r/r 10-128-1:	$UpCase
r/r 10-128-2:	$UpCase:$Info
r/r 3-128-3:	$Volume
r/r 64-128-2:	ps5.jpg
V/V 65:	$OrphanFiles
```

From the output I saw that there was another jpg picture so I extracted it as well
using **TSK - fcat**

```
fcat ps5.jpg linuxfs_extracted.dd > ps5jpg_extracted.jpg
```

![](./assets/ps5jpg_extracted.jpg)

