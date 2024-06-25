## NOTES FROM VARIOUS RESOURCES
## Miro Board

[Miro Board direct link](https://miro.com/app/board/uXjVK6q4wf0=/?share_link_id=512001525084)

## Root keys to examine in a forensic examination

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

### Jumplists

They are designed to improve the taskbar productivity  providing quick access to the most recently or frequently used file, tasks and application-specific functions.

They are stored in two primary locations :
- **AutomaticDestinations** : that contains automatically generated Jump Lists
	- PATH : **\Users\<Username>\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations**

- **CustomDestinations** : that contain custom Jump Lists created by applications
	- PATH : **C:\Users\<Username>\AppData\Roaming\Microsoft\Windows\Recent\CustomDestinations**

Jumplists are user-specific so the analysis process has to be repeated for every user on the machine.
They can link to files that have been deleted and can store useful informations such as :
- File Paths
- Last Access timestamps
- Application-specific information


### Shellbags

Are a set of registry keys in Windows that **store user preferences** for folder views.
These preferences are metadata record about folder the user interacted with ( position on the screen, timestamps... ) even if they were deleted from the system.

**They store evidence about resources in the local host, network share or on removable devices**.

Visible from :
1. **Explorer Access**
	- HKEY_USERS\\Local Settings\\Software\\Microsoft\\Windows\\Shell\\Bags
	- HKEY_USERS\\Local Settings\\Software\\Microsoft\\Windows\\Shell\\BagMRU
2. **Desktop Access**
	- HKEY_USERS\\Software\\Microsoft\\Windows\\Shell\\BagMRU
	- HKEY_USERS\\Software\\Microsoft\\Windows\\Shell\\Bags

### USB Device Analysis

Is possible to achieve a lot of information via **shell items** and **SYSTEM / SOFTWARE / NTUSER.DAT Registries** :
- HKEY_LOCAL_MACHINE\\SYSTEM\\Current COntrol Set\\Enum\\USB
- HKEY_LOCAL_MACHINE\\SYSTEM\\MountedDevices
- HKEY_LOCAL_MACHINE\\SYSTEM\\Current Control Set\\ENUM\\USBSTOR

Possible information that can be obtained are:
- Installation date and first connection
- Last connection
- Last Removal
- Volume Serial Number

### Prefetch

Are used to increase system performances by pre-loading libraries/code needed by applications.
They store also useful information like the first and last time an application was run as well as the number of times.
Stored in **.pf** files in **C:\Windows\Prefetch** and there is one file per executable.

### Internet Browsers

Is possible to recover:
- Cache files
- History
- Cookies
- Filled forms
- Last Session
- Last Tabs
- Login Data

User data is stored in **C:\\Users\\username\\AppData\\Local\\Google\\Chrome\\User Data\\Default
