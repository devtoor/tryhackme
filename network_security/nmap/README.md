# Nmap

Learn how to leverage the Nmap network scanner to discover live hosts and open ports using basic and advanced scan options.

---

In this module, we will learn how to utilise the Nmap scanner to discover live hosts and scan them for open ports. You will gain a deep knowledge of the various Nmap port scans, from TCP connect and stealth (SYN) port scans to null, FIN, Xmas and idle host (zombie) port scans. We will explore in detail the advanced options, including packet fragmentation, source address spoofing, and decoys. We will learn to use OS and service detection and how to leverage the power of Nmap scripts. Finally, we demonstrate the different ways to save Nmap scan results for future reference.

## Searching for Scripts

[Nmap NSE doc](https://nmap.org/nsedoc/)

```bash
grep <srv|cat> /usr/share/nmap/scripts/script.db
```

```bash
ls -l /usr/share/nmap/scripts/<srv>*
```

## Installing New Scripts

```bash
sudo apt update && sudo apt install nmap
```

```bash
sudo wget -O /usr/share/nmap/scripts/<script-name>.nse https://svn.nmap.org/nmap/scripts/<script-name>.nse

nmap --script-updatedb
```
