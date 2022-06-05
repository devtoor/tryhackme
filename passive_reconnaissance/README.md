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
| Lookup WHOIS record | `whois <domain.tld>` |
| Lookup DNS A records | `nslookup -type=A <domain.tld>` |
| Lookup DNS MX records at DNS server | `nslookup -type=MX <domain.tld> <dns-server>` |
| Lookup DNS TXT records | `nslookup -type=TXT <domain.tld>` |
| Lookup DNS A records | `dig <domain.tld> A` |
| Lookup DNS MX records at DNS server | `dig @<dns-server> <domain.tld> MX` |
| Lookup DNS TXT records | `dig <domain.tld> TXT` |

Resources:

[DNSDumpster](https://dnsdumpster.com/)

[Shodan.io](https://www.shodan.io/)
