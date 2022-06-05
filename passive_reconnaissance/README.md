# Passive Reconnaissance

## nslookup and dig

| Query type | Result |
| :----: | :----: |
| A | IPv4 Addresses |
| AAAA | IPv6 Addresses |
| CNAME | Canonical Name |
| MX | Mail Servers |
| SOA | Start of Authority |
| TXT | TXT Records |

| Purpose | Commandline Example |
| :----: | :----: |
| Lookup WHOIS record | `whois tryhackme.com` |
| Lookup DNS A records | `nslookup -type=A tryhackme.com` |
| Lookup DNS MX records at DNS server | `nslookup -type=MX tryhackme.com 1.1.1.1` |
| Lookup DNS TXT records | `nslookup -type=TXT tryhackme.com` |
| Lookup DNS A records | `dig tryhackme.com A` |
| Lookup DNS MX records at DNS server | `dig @1.1.1.1 tryhackme.com MX` |
| Lookup DNS TXT records | `dig tryhackme.com TXT` |

Resources:

[DNSDumpster](https://dnsdumpster.com/)

[Shodan.io](https://www.shodan.io/)
