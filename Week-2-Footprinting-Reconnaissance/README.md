# PENETRATION TESTING REPORT

## Week 2: Footprinting, Reconnaissance & Network Scanning

**Student Name:** Aditi Pashte 
**Operating System:** Windows 11  
**Virtualization Environment:** Oracle VirtualBox  
**Date of Lab Work:** 18–19 November 2026

---

## 1. Liability Disclaimer

This report is prepared for educational and cybersecurity lab practice purposes only.

All network scanning activities described for the local network were performed on the student's own network environment.

The website reconnaissance results are documented as observations from the commands executed during the lab. This report does not claim current authorization to test the external website. Any further active testing must be performed only with valid written permission from the website owner.

No exploitation, brute-force attacks, or unauthorized access attempts are included in this report.

## 2. Introduction

The objective of this lab was to understand the basic concepts of penetration testing, footprinting, reconnaissance, and network scanning.

The practical exercises involved collecting publicly observable domain information, identifying web technologies, examining DNS records, checking web security mechanisms, and discovering live hosts on a local network using Zenmap.

The lab also included a localhost TCP scan to understand how Nmap identifies open and filtered ports on a Windows computer.

## 3. Tools Used

| Tool | Purpose |
|---|---|
| WHOIS | Domain registration information |
| WhatWeb | Website technology identification |
| nslookup | DNS lookup |
| curl | HTTP response header inspection |
| WAFW00F | Web Application Firewall detection |
| DNSRecon | DNS record enumeration |
| Nmap | Network scanning |
| Zenmap | Graphical network scanning |
| Command Prompt | Network configuration and commands |

## 4. Activities Performed

### 4.1 Footprinting & Reconnaissance

The following commands were used to collect domain and website information.

**A. WHOIS Lookup**

Command:

```bash
whois networkwalks.com
```

Observations:

- Domain: networkwalks.com
- Registrar: GoDaddy.com, LLC
- Creation Date: 06 November 2019
- Expiry Date: 06 November 2027
- Domain privacy protection: Domains By Proxy, LLC.
- DNSSEC: Unsigned
- Nameserver information included HostGator nameservers in the WHOIS output.

**B. WhatWeb Scan**

Command:

```bash
whatweb networkwalks.com
```

Observations:

- Website redirected from HTTP to HTTPS.
- HTTPS response: 200 OK
- Web server identified as Apache.
- Website title: Networkwalks Academy
- WordPress detected.
- Bootstrap detected.
- jQuery version: 3.7.1
- Google Tag Manager detected.
- WP Download Manager detected.

**C. DNS Lookup**

Command:

```bash
nslookup networkwalks.com
```

Observations:

- DNS Server: 8.8.8.8
- Domain resolved to IP address: 192.232.216.135
- Response was non-authoritative.

**D. HTTP Header Inspection**

Command as executed:

```bash
curl -I https:///networkwalks.com
```

Observations:

- HTTP response: 200 OK
- Web server: Apache
- Content type: text/html; charset=UTF-8
- HTTP response headers included security-related policy information.
- A cookie was observed with Secure and HttpOnly attributes.

Note: The command was entered with three slashes after `https:`. The output is recorded as received.

**E. WAF Detection**

Command:

```bash
wafw00f networkwalks.com
```

Observations:

- WAFW00F version: 2.4.2
- Detected Web Application Firewall: ModSecurity (SpiderLabs)
- Two HTTP requests were made during detection.

**F. DNS Enumeration**

Command:

```bash
dnsrecon -d networkwalks.com
```

Observations:

- SOA record identified.
- Nameserver records identified.
- A record resolved to 192.232.216.135.
- MX record identified as mail.networkwalks.com.
- SPF TXT record was found.
- Google site verification TXT record was found.
- SRV records for email autodiscovery were identified.
- DNSSEC query returned no answer.

DNSRecon reported 8 records found.

### 4.2 Network Scanning with Zenmap

The local network was scanned using Zenmap on Windows 11.

**A. Finding Local IP Address**

Command:

```cmd
ipconfig
```

Network details:

| Parameter | Value |
|---|---|
| Active Adapter | Intel Wi-Fi 6 AX203 |
| IPv4 Address | 192.168.43.216 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.43.1 |
| Network Range | 192.168.43.0/24 |

**B. Ping Scan Using Zenmap**

Target:

```text
192.168.43.0/24
```

Scan Profile: Ping Scan

The scan identified two live hosts on the local network.

Results:

| IP Address | Host Status |
|---|---|
| 192.168.43.1 | Up |
| 192.168.43.216 | Up |

Total IP addresses scanned: 256  
Live hosts found: 2  
Scan duration: 2.68 seconds

The scan was used to identify live hosts. It did not perform a port or service scan.

**C. MAC Address Identification**

The Zenmap output identified the gateway MAC address. The laptop's MAC address was obtained using:

```cmd
ipconfig /all
```

Results:

| Device | IP Address | MAC Address |
|---|---|---|
| Gateway | 192.168.43.1 | 22:50:C4:84:35:3F |
| Laptop | 192.168.43.216 | 90-10-57-41-39-67 |

**D. Network Topology**

The Zenmap Topology tab was used to view the network topology.

The topology graphic was saved in PDF format as required by the lab instructions.

**E. Additional Localhost TCP Scan**

An additional scan was performed on the local Windows computer.

Command:

```cmd
nmap -sT 127.0.0.1
```

Results:

- Target: 127.0.0.1
- Host status: Up
- Open ports: 135/tcp and 445/tcp
- Filtered ports: 998
- Nmap version: 7.70
- Scan duration: 47.58 seconds

Open ports identified:

| Port | State | Service |
|---|---|---|
| 135/tcp | Open | MSRPC |
| 445/tcp | Open | Microsoft-DS |

This was an additional localhost scan and was separate from the Zenmap ping scan.

## 5. Risk Analysis / Impact

The following observations were recorded during the lab.

| Observation | Risk Level | Explanation |
|---|---|---|
| DNSSEC unsigned | Informational | DNSSEC was not enabled according to the observed lookup results. This alone does not prove a vulnerability. |
| ModSecurity detected | Positive Security Control | A web application firewall was detected. |
| HTTP to HTTPS redirect | Positive Security Control | The website redirected HTTP traffic to HTTPS. |
| Ports 135 and 445 open on localhost | Informational | Windows RPC and SMB-related services were listening locally. Their exposure to other networks was not tested. |
| Two live local network hosts | Informational | Two hosts responded to the ping scan. |

No exploitation or vulnerability validation was performed. Therefore, these observations should not be treated as confirmed exploitable vulnerabilities.

## 6. Recommendations

1. Consider enabling DNSSEC where supported and properly configured.
2. Keep WordPress, plugins, themes, and web server software updated.
3. Review firewall rules and restrict access to Windows services such as ports 135 and 445 where appropriate.
4. Continue using HTTPS for website communication.
5. Maintain and regularly review web application firewall rules.
6. Monitor DNS records and domain registration information.
7. Perform security assessments only with valid written authorization.

## 7. Conclusion

This lab provided practical experience with footprinting, reconnaissance, DNS enumeration, website technology identification, and network scanning.

WHOIS, WhatWeb, nslookup, curl, WAFW00F, and DNSRecon were used to collect domain and website information.

Zenmap was used to scan the local network and identify two live hosts. The IP addresses and MAC addresses were recorded, and the network topology was saved in PDF format.

An additional Nmap TCP scan was also performed on localhost, identifying two open ports and 998 filtered ports.

Overall, the lab helped develop an understanding of basic network reconnaissance and scanning techniques.

## 8. Evidences Collected

The following evidence files should be placed in the `Screenshots` folder or `Report` folder as appropriate.

| Evidence | Description |
|---|---|
| Week2-Zenmap-Topology.png | Zenmap network topology screenshot |
| Week2-Zenmap-Scan-Result.png | Zenmap ping scan results |
| Topology PDF | Saved network topology in PDF format |
| WHOIS Screenshot | Domain registration information |
| WhatWeb Screenshot | Website technology detection |
| DNSRecon Screenshot | DNS enumeration results |
| WAFW00F Screenshot | WAF detection result |
| Nmap Localhost Screenshot | Additional localhost TCP scan |
| ipconfig-all Screenshot | Local adapter and MAC address information |

**End of Report**
