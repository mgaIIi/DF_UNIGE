## Outline
Is a proprietary journaling file system where **everything is a file except the Volume Boot Record**.
**Generic data structures** embed specific content with a very **scalable design**.

Each unit of information associated with a file is implemented as a **file attribute** that consists of a **stream**. A file attribute can be its content, name or time-stamps.
There **$DATA attribute** corresponds to the file content.
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

A directory contains a sorted list of the files.
For large directories the names are stored in 4kb fixed-size index buffers.

Index buffers implement a B-tree data-structure which minimizes the number of disk accesses per file find.

Each directory entry contains:
- the record number in the **Master File Table**
- **Time stamp** informations
- **file size**

NTFS duplicates data to speed up directory browsing at the cost of updating those information in two places


