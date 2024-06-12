They are stored in files know as **hives** and they contain keys and values.
A live interaction with registries can happen via the command **regedit**.
Values can be :
- Strings
- Binary data
- Integers
- Lists

## System Hives
Are stored in %WINDIR%\\System32\\Config\\ :
- **SAM** : Contains local users' informations 
	- Username
	- User RID ( Relative Identifier)
	- Account Creation timestamp
	- Last login
	- Last failed login
	- Last password change
	- Password "hint"
	- Group Information
- **SECURITY**
- **SOFTWARE**
- **SYSTEM**
## Users Hives
-  %USERS%\\username\\NTUSER.DAT
-  %USERS%\\username\\AppData\\Local\\Microsoft\\Windows\\USERCLASS.DAT
## Operating system version
-  SOFTWARE \\Microsoft\\Windows NT\\CurrentVersion
## Computer Name
-  SYSTEM \\Current Control Set\\Control\\ComputerName\\ComputerName

## Timezone
- SYSTEM \\Current Control Set\\Control\\TimeZoneInformation
## Services
- SYSTEM \\CurrentControlSet\\Services
## Network TCP/IP Parameters
- SYSTEM \\CurrentControlSet\\Services\\Tcpip\\Parameters\\Interfaces
## Network List
- SOFTWARE \\Microsoft\\Windows NT\\CurrentVersion\\NetworkList
## Installed Applications
- SOFTWARE \\Microsoft\\Windows\\CurrentVersion\\Uninstall
## Shutdown Time
- SYSTEM \\Current Control Set\\Control\\Windows
## Autorun
- SOFTWARE \\Microsoft\\Winows\\CurrentVersion\\Run

## User Activities
## Installed Applications
- NTUSER.DAT \\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Uninstall\\
## RecentDocs
- NTUSER.DAT \\Software\\Microsoft\\Windows\\CurrentVersion\\Explorer\\RecentDocs
## LastVisited MRU
- NTUSER.DAT \\Software\\Miscorosf\\Windows\\CurrentVersion\\Explorer\\ComDlg32\\LastVisitedPidlMRU
# OpenSave MRU
- NTUSER.DAT \\Software\\Miscorosf\\Windows\\CurrentVersion\\Explorer\\ComDlg32\\OpenSavePidlMRU
# UserAssist
- NTUSER.DAT \\Software\\Microsoft\\Windows\\CurrentVersion\\Explorer\\UserAssist

## LNK files / Shortcut Files
- **Shell Item** : A dara or a file containing information to access another file
- **LNK File/Shortcut**: A shell item saved in a file wifh LNK extension:
	- A shortcut to execute an application on User's Desktop
	- A shortcut to open a file automatically created

A LNK file can contain:
- Target drive type
- Path of target file
- Target file MAC timestamps
- Target file size

## Recent Files
LNK files are automatically created by Windows in a Recent folder:
- **User based folder** : %USERS%\\username\\AppData\\Roaming\\Microsoft\\Recent

- LNK File Creation Time : first time file by thatn name was opened
- LNK File Modified Time : Last time file by that name was opened

## Jumplists
Can contain much more references to opened files than "RecentDocs" and "Recent" folder.
Two subfolder:
- AutomaticDestinations
- CustomDestinations
Creation Time -> first time of app execution
Modified Time -> last time of app execution
