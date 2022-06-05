# Nmap

Learn how to leverage the Nmap network scanner to discover live hosts and open ports using basic and advanced scan options.

## Nmap Live Host Discovery

| Scan TYpe | Example Command |
| :----: | :-----: |
| ARP Scan  | `sudo nmap -PR -sn <MACHINE_IP>/24` |
| ICMP Echo Scan | `sudo nmap -PE -sn <MACHINE_IP>/24` |
| ICMP Timestamp Scan | `sudo nmap -PP -sn <MACHINE_IP>/24` |
| ICMP Address Mask Scan | `sudo nmap -PM -sn <MACHINE_IP>/24` |
| TCP SYN Ping Scan | `sudo nmap -PS22,80,443 -sn <MACHINE_IP>/24` |
| TCP ACK Ping Scan | `sudo nmap -PA22,80,443 -sn <MACHINE_IP>/24` |
| UDP Ping Scan | `sudo nmap -PU53,161,162 -sn <MACHINE_IP>/24` |

> - Add `-sn` if you are only interested in host discovery without port-scanning. Omitting `-sn` will let Nmap default to port-scanning the live hosts.

| Option | Purpose |
| :----: | :----: |
| `-n` | no DNS lookup |
| `-R` | reverse-DNS lookup for all hosts |
| `-sn` | host discovery only |

## Nmap Basic Port Scans

| Port Scan Type | Example Command |
| :----: | :----: |
| TCP Connect Scan | `nmap -sT <MACHINE_IP>` |
| TCP SYN Scan | `sudo nmap -sS <MACHINE_IP>` |
| UDP Scan | `sudo nmap -sU <MACHINE_IP>` |

> - These scan types should get you started discovering running TCP and UDP services on a target host.

| Option | Purpose |
| :----: | :----: |
| `-p-` | all ports |
| `-p1-1023` | scan ports 1 to 1023 |
| `-F` | 100 most common ports |
| `-r` | scan ports in consecutive order |
| `-T<0-5>` | -T0 being the slowest and T5 the fastest |
| `--max-rate 50` | rate <= 50 packets/sec |
| `--min-rate 15` | rate >= 15 packets/sec |
| `--min-parallelism 100` | at least 100 probes in parallel |

> - `-T4` is often used during CTFs and when learning to scan on practice targets, whereas `-T1` is often used during real engagements where stealth is more important.
## Nmap Advanced Port Scans

| Port Scan Type | Example Command |
| :----: | :----: |
| TCP Null Scan | `sudo nmap -sN <MACHINE_IP>` |
| TCP FIN Scan | `sudo nmap -sF <MACHINE_IP>` |
| TCP Xmas Scan | `sudo nmap -sX <MACHINE_IP>` |
| TCP Maimon Scan | `sudo nmap -sM <MACHINE_IP>` |
| TCP ACK Scan | `sudo nmap -sA <MACHINE_IP>` |
| TCP Window Scan | `sudo nmap -sW <MACHINE_IP>` |
| Custom TCP Scan | `sudo nmap --scanflags [URG-ACK-PSH-RST-SYN-FIN] <MACHINE_IP>` |
| Spoofed Source IP | `sudo nmap -S <SPOOFED_IP> <MACHINE_IP>` |
| Spoofed MAC Address | `--spoof-mac <SPOOFED_MAC>` |
| Decoy Scan | `nmap -D <DECOY_IP>,<ME> <MACHINE_IP>` |
| Idle (Zombie) Scan | `sudo nmap -sI <ZOMBIE_IP> <MACHINE_IP>` |
| Fragment IP data into 8 bytes | `-f` |
| Fragment IP data into 16 bytes | `-ff` |

> - Scan `-sN`, `-sF` and `-sX`  can be efficient is when scanning a target behind a stateless (non-stateful) firewall. A stateless firewall will check if the incoming packet has the SYN flag set to detect a connection attempt. Using a flag combination that does not match the SYN packet makes it possible to deceive the firewall and reach the system behind it.
> - ACK `-sA` and window `-sW` scans are exposing the firewall rules, not the services.
> - Scan types rely on setting TCP flags in unexpected ways to prompt ports for a reply. Null `-sN`, FIN `-sF`, and Xmas `-sX` scan provoke a response from closed ports, while Maimon `-sM`, ACK `-sA`, and Window `-sW` scans provoke a response from open and closed ports.

| Option | Purpose |
| :----: | :----: |
| `--source-port <PORT_NUM>` | specify source port number |
| `--data-length <NUM>` | append random data to reach given length |
| `--reason` | explains how Nmap made its conclusion |
| `-v` | verbose |
| `-vv` | very verbose |
| `-d` | debugging |
| `-dd` | more details for debugging |
