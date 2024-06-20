## Miro Board

![Public viewer access](https://miro.com/app/board/uXjVK6q4wf0=/?share_link_id=512001525084)

They are stored in files know as **hives** and they contain keys and values.
A live interaction with registries can happen via the command **regedit**.
Values can be :
- Strings
- Binary data
- Integers
- Lists

## Root keys

1. **HKEY_CLASSES_ROOT**
	- Purpose: Stores information about file associations and COM objects
	- Details: This key is used to determine how files with certain extensions are handled by Windows, which program are used to open them, and the properties and actions available for these file types. 

2. **HKEY_CURRENT_USER**
	- Purpose: Contains cofiguration settings and preferences for the currently logged-in user
	- Details: This includes user-specific setttings like desktop background, screen saver, folder views, and application settings. It maps to the user's profile directory. It maps to the user's profile directory

3. **HKEY_LOCAL_MACHINE**
	- Purpose: Contains settings and configuration information for the local computer, applying to all users.
	- Details: This includes hardware settings, system-wid software settings, and system configuration. It is divided into several subkeys, such as:
		- **HARDWARE** : Informailn about hardware currently detected in the system
		- **SAM** : Security Account Manager database, which stores information about user accounts
		- **SECURITY** : Security-related information, including local security policies
		- **SOFTWARE** : Installed software and configuration settings
		- **SYSTEM** : System-related information, such as control sets and services

4. **HKEY_USERS**
	- Purpose: Contains user-specific settings fro all users on the computer
	- Details: Each user account on the system has a subkey in HKU. The currently logged-in user's settings are mirrored in HKCU.

5. **HKEY_CURRENT_CONFIG**
	- Purpose: Contains information about the current hardware profile used by the computer at startup
	- Details: This includes settings that are specific to the current hardware configuration, such as display settings and printer settings.

6. **HKEY_PERFORMANCE_DATA**
	- Purpose: Provides runtime performance data
	- Details: This key is used by performance monitoring applications to access system performance data.		

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

## Shellbags
Are used to store user preferences for folder display in windows explorer
**Useful to determine which folders were accessed on the local host, on a network share or on a removable device**
For some folders is also possible to determine when they were accessed and can contain evidence of previously existing folders

Stored in user's registry files:
**Explorer Access**
- USRCLASS.DAT \\Local Settings\\Software\\Microsoft\\Windows\\Shell\\Bags
- USRCLASS.DAT \\Local Settings\\Software\\Microsoft\\Windows\\Shell\\BagMRU
**Desktop Access**
- NTUSER.DAT \\Software\\Microsoft\\Windows\\Shell\\BagMRU
- NTUSER.DAT \\Software\\Microsoft\\Windows\\Shell\\Bags

## USB Device Analysis
Is possible to achieve a lot of informations via **shell items** and **SYSTEM / SOFTWARE / NTUSER.DAT Registries** :
- SYSTEM \\Current COntrol Set\\ENUM\\USB
- SYSTEM \\Current Control Set\\ENUM\\USBSTOR
Possible informations that can be obtained are:
- Installation date and first connection
- Last connection
- Last Removal
- Volume Serial Number

## Prefetch
Are used to increase system performances by preloading libraries/code.
Stored in .pf files in **C:\\Windows\\Prefetch** and there is one file per executable.

## Internet Browsers
Is possible to recover:
- Cache files
- History
- Cookies
- Filled forms
- Last Session
- Last Tabs
- Login Data
User data is stored in **C:\\Users\\username\\AppData\\Local\\GOogle\\Chrome\\User Data\\Default





