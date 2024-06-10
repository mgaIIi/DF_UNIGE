## Outline
Based on traditional UNIX FS but with modern features like:
- extended-attributes
- journaling
- encryption
- 64-bit support

Designed to be *extensible* via optional features:
- *compatible features* : if not supported the OS can still mount the FS
- *incompatible features* : if not supported the FS should not be mounted
- *read only compatible features* : if not supported the OS can still read only mount the FS 

## ext4
Principle features : 
- *Supports very large volumes* with a max files size from 2TB to 16TB
- *Backward compatible* with the ability to mount ext3 file-systems
- *Extents for more efficient data-block mapping* like NTFS

Layout:
- sectors are grouped into *blocks* of variable size that *are not split into fragments*
- the *SuperBlock* at offset 1024 and of size 1024 bytes contains :
	- File System Parameters
	- Optional reserved area
- the boot code, *if present*, is contained in the Master Boot Record, MBR
- the remainder of the File System is divided into *block groups*:
	- contain *data bitmap*, *inode bitmap*, *inode table*, *data blocks*
	- may contain a backup of the Super Block and *group descriptors table*
- the main *group descriptor table* is located in the block after the *Super Block* and a backup can be found in every block group after the Super Block backup or only some if the *sparse superblock option* is set

> *dumpe2fs* is the command to print superblock informations and block groups informations

#### Places to hide data
- The first 1024 bytes are not technically used
- There are unused bytes in the Super Block
- There can be unused entries in the Group Descriptor Table
- There are reserved Group Descriptor Table blocks

*Inodes* are the primary metadata structure and they:
- *have a fixed size* but extra space can be taken by extended attributes
- *have address* starting at 1 ( 1..10 are reserved -> **1 bad blocks** , **2 root dir**, **8 journal** )
- *are stored in table* for each block group
- *have a allocation status* determined using a *inode bitmap*

*Directories* are a data structure containing:
- *the file name* -> with one byte iused to store the file type
- *the inode number associated to the file name*

*Hash trees* can be used in ext3 and ext4 to speed up name lookups:

*Extended attributes* are a list of name/value pairs with specific functions to get and set them. They are store in a block allocated to the file