---
title: 'Hacking SAM'
description: 'Hacking Sam Databases'
heroImage: ''
published: false
pubDate: '10-26-2024'
tags: ['windows', 'password hacking', 'hacking']
---


There's three registry hives that are important when it comes to attacking SAM. The three hives
are listed below.

* hklm/sam *(Contains the local account password hashes)*
* hklm/system *(Contains the system bootkey used to encrypt the sam databases)*
* hklm/security *(Contains cached credentials for domain accounts)*

## Hive System File Paths

All the hives stated above can be located in the following folder directories followed by the hive name. ex: sam, system, security. 

```ps
C:\Windows\System32\config\(hive name)
```

## We Can't Copy Hives directly

We cannot directly copy the hives from their directory locations because these files are always in use by the system. Registry hives such as *SAM*, *SECURITY*, and *SYSTEM* are located in the `C:\Windows\System32\config` directory and are constantly accessed by the operating system for crucial tasks. Copying these files directly can lead to incomplete or corrupted backups due to these active locks.

## Handling System Locks:

Accessing these hives through *HKLM* using *reg.exe* ensures the data is properly handled. *reg.exe* deals with all the system locks and complexities, allowing us to copy the data without running into issues. Directly copying from `C:\Windows\System32\config` can be problematic because these files are constantly in use and locked by the system

## Mounting data to HKLM

When Windows runs, it mounts files like *SAM*, *SYSTEM*, and *SECURITY* to specific places in the registry *(e.g., HKLM\SAM)*. This means that while these files are stored on disk in the *config* directory, they are actively managed and used by the system in the registry. By accessing `HKLM`, we interact with the data as the system uses it, ensuring that our backups are accurate and safely captured. This mounting process is what allows Windows to efficiently read and write settings, keeping the system running smoothly.
#### Accessing HKLM Using `reg.exe`

Once again we use *reg.exe* to safely access registry hives through *HKLM* (HKEY_LOCAL_MACHINE) because it allows us to interact with the data as the system uses it, making sure our backups are safe and without causing any problems to the system. *reg.exe* is a command-line tool built into Windows that helps perform various operations on the Windows Registry, including adding, deleting, querying, exporting, and importing registry data. For example, to back up the *SAM* hive, you would use:

```
reg save HKLM\SAM C:\Backup\SAM.save
```

This command saves the *SAM* data from the registry to a file named *SAM.save* in the *(C:\Backup)* directory, ensuring it’s accurate and safely copied.