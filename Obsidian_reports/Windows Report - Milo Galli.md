

```
Build a timeline of events for the HD01.E01 forensic image

Submit

    A spreadsheet with a detailed timeline
    A document explaining what happened on the system in each day in which it was used

The spreadsheet must contain, at least, the following elements:

    MFT (for relevant files)
    System Registry
    User Registry
    LNK files
    Jumplists
    Shellbags
    USB device analysis
    Prefetch
    Google Chrome
```

# Useful concepts
HKCU -> HKey Current User
HKLM -> HKey Local Machine
Windows registry is a giant database, the registry itself can be found at \windows\system32\config ( everything is protected by the os ).
SAM
SECURITY
SOFTWARE
SYSTEM

Every user has a NTUSER.DAT and that replace the HKCU when looking at the hives
Running regedit -> we can take a look at the registries

### SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer
#### \ComDlg32
- LastVisitedPidlMRU --> when we see that a path where we have saved the last element. That path is stored here -> it can show paths that have been deleted 
- OpenSavePidlMRU
#### \RecentDocs
Pretty common one, Recent linkfiles that have been interacted with not long ago
#### \RunMru
If something is typed in the run prompt we are going to find what was written here --> e.g. the last application used can be seen here
#### \TypedPaths
If we type explicitly in the file explorer a paht we are going to find it here. This can be useful because it means that the user knows exactly what he was looking for.
also this can reveal paths that where deleted.
#### \UserAssist
It will show what particular program was executed, when it was executed, how many times it was executed and also we know the user from the NTUSER.DAT.
The entrys are encrypted int ROT13 though.

### HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\[Run,RunOnce]
### HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\[Run,RunOnce]

They are one of 51 locations where it can be specified for a program to run upon logging
Another location is Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run

### Shellbags

#### HKCU\SOFTWARE\Microsoft\Windows\Shellbags --> ShellbagExplorer
#### \BagMRU
#### \Bags

Whenever you customized the look and the feel of files and folder, the position, the icons, the sorting method of folders and other ( mostly graphical stuff ),
those are saved in shellbags. They are very useful because the can maintain informations about content that has been deleted a long ago.
So from them it's possible to recreate entire trees of files and demonstrate that paths actually existed on the machine at a given point in time.

### HKCU\SOFTWARE\Classes
-> %USERPROFILE%\AppData\Local\Microsoft\Windows\UsrClass.dat

It was added in Windows 7 and its main purpose is due to segmentation for registry hives.
Using FTKimager you can obtain protected files with a button but it doesn't take USERCLASS.dat.

### USB DEVICES
#### HKLM\SYSTEM\CurrentControlSet\Enum\USBSTOR
#### HKLM\SYSTEM\CurrentControlSet\Enum\USB

CurrentControlSet can be seen only on a live machine and there may be more than one.
Looking at the correct key identified the CurrentControlSet it's possible to identify the correct one.
We can take the serial number of USB connected devices but if the manifacturer of the USB device didn't provide a globally unique serial number, Windows will provide one and assign it.

#### HKLM\SOFTWARE\MicrosoftWindows Portable Devices\Devices 
Here we can find additional informatons about USB devices like the friendly name

#### HKLM\SYSTEM\MountedDevices
We can find the device's Volume Globally Unique Identifier ( Volume GUID ) and also we can see the drive letter that was assigned (e.g. E:\...)
(Nearsoft softwares to get info about them)

#### HKLM\SOFTWARE\MicrosoftWindows NT\CurrentVersion\EMDMgmt
This key will ONLY be present if the system drive is not SSD
It was traditionally used for readyboost
We can find here the Volume Serial Number of the USB device
Using the Volume GUID found in SYSTEM\MountedDevices it's possible to find the user that actually mounted the USB device.

### NTUSER.DAT\SOFTWARE\MicrosoftWindows\CurrentVersion\Explorer\Mountpoints2
Here we can find the first time that a USB device was connected, last time and the removal time



















