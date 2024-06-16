## Outline
Is a proprietary journaling file system where **everything is a file except the Volume Boot Record**.
**Generic data structures** embed specific content with a very **scalable design**.

Each unit of information associated with a file is implemented as a **file attribute** that consists of a **stream**. A file attribute can be its content, name or time-stamps.
The **$DATA attribute** corresponds to the file content.
Applications can create additional named streams called **Alternate Data Streams**.

When mounting you specify :
- **show_sys_files** to show metafiles in directory listing
- **streams_interface=windows** to access **Alternate Data Streams** like in Windows

**Hard links** allow multiple paths to refer to the same file
**Soft/Symbolic Links** are strings that interpreted dynamically **can point to files/directories and non-existing things**, implemented as reparse points.

**NTFS maps the whole volume into clusters** :
- the **default cluster factor** is always a power of 2 and have variable size.
- **logical cluster numbers** correspond to numbering clusters from the beginning of the volume
- **Virtual cluster numbers** is used to address data within a file

The **Master File Table is an array of file records**:
- **record size** is defined at format time
- the base file record stores the location of the others if a file needs more metadata space
The **Master File Table** location is defined in the BIOS Parameter Block in the **Volume Boot Record** 
The **Master File Table** contains  one record for each file including itself.

Files are **identified by 64 bit 'file record numbers'**  which consist of :
- a **file number** that is the position in the **Master File Table**
- a **sequence number** that is incremented when a file record is re-used

When the value of a file's attribute is stored in the **Master File Table** is called a **resident attribute** and every attribute starts with a standard header.

When a value is too large to be stored in a **Master File Table** record a cluster is allocated outside it and each contiguous group of clusters is called a **run/extent**.
Runs can form **run-lists** with a variable size.

When file metadata can't fit in a single **Master File Table record** other ones are used and the **attribute list** attribute is added to contain:
- name and type code of each attribute
- the file number of the **Master File Table** record where the attribute is located

**Fixups** are used by Windows operating systems to ensure the integrity of the file system's critical metadata. They help detect and correct issues due to poser failures or hardware's malfunctions.

NTFS divides the file's data into **compression units 16 clusters long** that is a trade-off between producing smaller compressed files and slowing random-access read operations.

NTFS duplicates data to speed up directory browsing at the cost of updating those information in two places

## NOTES FROM THE BOOK

A file-system that is designed for scalability where generica data structures wrap other data structures with specific content.
One example of generic wrapper is that every byte of data in NTFS is allocated to a file, even file-system administrative data that are typically hidden.
The entire file-system is considered a data area.
The only consinstent layout is that the first sectors contain the boot sector and the boot code.

The **Master File Table** contains information about all the files and directories.
Every file and direcory has at least one entry in the table and they are very simple:
- 42 bytes for a defined purpose
- the rest for attributes, that are small data structures storing specific data ( file's name, size and even the content )

![](./assets/MFT_ENTRY.png)

The MFT is a file, like everything else, so it has an entry for itself that is the first one ( **$MFT** ) and describes the on-disk location of the MFT.
The MFT size grows sequentially and the addresses are called **file numbers** and are useful to detect possible corruptions and to recover deleted content.

Every MFT entry has a little internal structure and most of it is used to store attributes, each of the has its own internal structure.
All attributes have two parts:
- header : identifies the attribute's type, size, name and stores its flags.
- content : can have any format and **size** ( it can be stored in the MFT entry itself or can be **non-resident** and be stored in a external cluster )

![](./assets/MFT_ENTRY_ATTRIBUTES.png)

![](./assets/MFT_ENTRY_ATTRIBUTES_NON_RESIDENT.png)


