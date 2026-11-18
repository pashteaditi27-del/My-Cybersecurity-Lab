# PENETRATION TESTING REPORT
## Week 2: Footprinting, Reconnaissance & Network Scanning

**Project:** Network Security Assessment  
**Target Domain:** `networkwalks.com`  
**Assessment Date:** 18 November 2026  
**Environment:** Kali Linux and Windows 11  
**Network Scanning Tool:** Zenmap / Nmap

---

## 1. Liability Disclaimer

This report has been prepared for educational and cybersecurity learning purposes. It documents the tool outputs and observations collected during the assessment.

The domain-related findings are based on the outputs supplied by the learner. The previous authorization letter shared for this project was valid from 17 August 2026 to 24 August 2026. It does not establish authorization for testing performed on 18 November 2026. No claim of current authorization is made in this report.

The network scan was performed against the learner's own mobile hotspot network.

No exploitation, brute-force attacks, password attacks, or unauthorized access to systems or data are documented in this report. Findings are limited to the evidence collected. Technology detection does not automatically confirm a vulnerability.

---

## 2. Introduction

Footprinting and reconnaissance are important initial phases of a penetration testing process. They help identify publicly available information about a domain, its DNS configuration, web technologies, server details, and security-related infrastructure.

The objective of this project was to practice reconnaissance tools and understand basic network discovery using Zenmap.

The assessment activities included:

- Collecting domain registration information using WHOIS.
- Identifying DNS records using nslookup and DNSRecon.
- Detecting website technologies using WhatWeb.
- Reviewing HTTP response headers using curl.
- Identifying the Web Application Firewall using WAFW00F.
- Discovering live hosts on the learner's own local network using Zenmap.

**Target domain:** `networkwalks.com`

**Resolved IP address:** `192.232.216.135`

---

## 3. Tools Used

| Tool | Purpose |
|---|---|
| WHOIS | Collecting domain registration information |
| nslookup | Resolving domain names to IP addresses |
| WhatWeb | Identifying website technologies |
| curl | Reviewing HTTP response headers |
| WAFW00F | Detecting Web Application Firewalls |
| DNSRecon | Enumerating DNS records |
| Zenmap | Visual network scanning interface |
| Nmap | Discovering live hosts on a network |
| Kali Linux | Reconnaissance and security testing environment |
| Windows 11 | Local network configuration and scanning environment |

### Tool Versions

| Tool | Version |
|---|---|
| WHOIS | 5.6.6 |
| WhatWeb | 0.6.4 |
| nslookup | 9.20.27-2 |
| WAFW00F | 2.4.2 |
| DNSRecon | 1.6.0 |
| Nmap | 7.70 |

---

## 4. Activities Performed

### 4.1 Footprinting & Reconnaissance

#### A. WHOIS Lookup

**Command used:**

```bash
whois networkwalks.com
```

**Findings:**

| Detail | Result |
|---|---|
| Domain Name | networkwalks.com |
| Registrar | GoDaddy.com, LLC |
| Creation Date | 06 November 2019 |
| Updated Date | 12 November 2025 |
| Expiry Date | 06 November 2027 |
| Registrar IANA ID | 146 |
| WHOIS Server | whois.godaddy.com |
| DNSSEC | Unsigned |
| Registrant Privacy | Domains By Proxy, LLC |

**Name servers observed:**

- NS6135.HOSTGATOR.COM
- NS6136.HOSTGATOR.COM
- NS29.DOMAINCONTROL.COM
- NS30.DOMAINCONTROL.COM

**Observation:**

The WHOIS lookup provided domain registration details, registrar information, registration dates, name servers, and DNSSEC status. Registrant information was privacy-protected.

#### B. DNS Lookup Using nslookup

**Command used:**

```bash
nslookup networkwalks.com
```

**Findings:**

| Detail | Result |
|---|---|
| DNS Server | 8.8.8.8 |
| DNS Server Address | 8.8.8.8 |
| Domain | networkwalks.com |
| Resolved IP | 192.232.216.135 |
| Response Type | Non-authoritative answer |

**Observation:**

The DNS lookup successfully resolved the domain name to `192.232.216.135`. This IP address matched the address identified by WhatWeb.

#### C. Website Technology Detection Using WhatWeb

**Command used:**

```bash
whatweb networkwalks.com
```

**Findings:**

| Detail | Result |
|---|---|
| Domain IP | 192.232.216.135 |
| HTTP Status | 301 Moved Permanently |
| HTTPS Status | 200 OK |
| Web Server | Apache |
| CMS | WordPress 7.1.4 |
| WordPress Download Manager | 3.3.77 |
| Bootstrap | 7.1.4 |
| jQuery | 3.7.1 |
| Page Title | Networkwalks Academy |
| Detected Email | info@networkwalks.com |
| Country Detected | United States |

**Observation:**

WhatWeb detected Apache as the web server and WordPress as the content management system. The HTTP request redirected to HTTPS, which returned a successful response.

These are technology fingerprints detected by WhatWeb. They do not independently confirm that any component is vulnerable.

#### D. HTTP Header Analysis Using curl

**Command used:**

```bash
curl -I https:///networkwalks.com
```

**Findings:**

| Header / Detail | Result |
|---|---|
| HTTP Status | 200 OK |
| Server | Apache |
| Content-Type | text/html; charset=UTF-8 |
| Referrer-Policy | no-referrer-when-downgrade |
| Cache Header | X-nginx-cache: WordPress |
| WordPress API | /wp-json/ |
| Cookie | __wpdm_client |
| Cookie Attributes | Secure, HttpOnly |

**Observation:**

The server returned an HTTP 200 OK response. The response headers exposed server information, WordPress REST API references, caching information, and cookie security attributes.

**Note:** The command was recorded exactly as supplied. The URL contains three slashes after `https:`. The result is documented as observed and should not be treated as a separate HTTPS configuration test.

#### E. Web Application Firewall Detection Using WAFW00F

**Command used:**

```bash
wafw00f networkwalks.com
```

**Findings:**

| Detail | Result |
|---|---|
| Tool | WAFW00F 2.4.2 |
| Target | https://networkwalks.com |
| WAF Detected | ModSecurity |
| Provider | SpiderLabs |
| Number of Requests | 2 |

**Observation:**

WAFW00F identified ModSecurity (SpiderLabs) as the Web Application Firewall protecting the website. This indicates that a web traffic filtering layer was detected.

#### F. DNS Enumeration Using DNSRecon

**Command used:**

```bash
dnsrecon -d networkwalks.com
```

**Findings:**

| Record Type | Result |
|---|---|
| SOA | ns6135.hostgator.com |
| SOA IP | 50.87.144.87 |
| NS | ns6135.hostgator.com |
| NS IP | 50.87.144.87 |
| NS | ns6136.hostgator.com |
| NS IP | 192.232.216.131 |
| MX | mail.networkwalks.com |
| MX IP | 192.232.216.135 |
| A | 192.232.216.135 |

**Additional DNS information:**

- DNS server version information was returned as `9.16.50-RH` for the two observed name server IPs.
- An SPF TXT record was detected.
- A Google site verification TXT record was detected.
- Eight SRV records were found for `_autodiscover._tcp.networkwalks.com`.
- The DNSSEC query returned an error: `No answer for DNSSEC query`.

**Observation:**

DNSRecon successfully identified SOA, NS, MX, A, TXT, and SRV records. The DNSSEC query did not return an answer, which is consistent with the unsigned DNSSEC status observed in WHOIS.

---

### 4.2 Network Scanning with Zenmap

The local network discovery exercise was performed using the learner's own mobile hotspot network.

#### A. Windows IP Configuration

**Command used:**

```cmd
ipconfig
```

**Findings:**

| Detail | Result |
|---|---|
| IPv4 Address | 192.168.43.216 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.43.1 |
| Network Range | 192.168.43.0/24 |

#### B. Zenmap Ping Scan

**Target entered:**

```text
192.168.43.0/24
```

**Profile selected:** Ping scan

**Nmap command equivalent:**

```bash
nmap -sn 192.168.43.0/24
```

**Scan findings:**

| Detail | Result |
|---|---|
| Network Range | 192.168.43.0/24 |
| Total IP Addresses Scanned | 256 |
| Hosts Reported Up | 2 |
| Scan Duration | 2.68 seconds |
| Nmap Version | 7.70 |

**Discovered hosts:**

| IP Address | Hostname | MAC Address | Status |
|---|---|---|---|
| 192.168.43.1 | Hotspot gateway | 22:50:C4:84:35:3F | Up |
| 192.168.43.216 | host.docker.internal | Not displayed | Up |

**Observation:**

The Zenmap ping scan discovered two live hosts in the selected network range. The first was the mobile hotspot gateway. The second was listed by Nmap as `host.docker.internal`.

The scan was a host discovery scan. It did not perform a port or service vulnerability assessment.

---

## 5. Risk Analysis / Impact

The following observations are based only on the collected reconnaissance output. They are not confirmed vulnerabilities.

| Finding | Potential Impact | Risk Level |
|---|---|---|
| Public domain registration information | Provides basic information about domain ownership and infrastructure | Low |
| Web server and CMS identification | Helps an attacker understand the website technology stack | Low |
| DNS records publicly discoverable | Reveals domain infrastructure and mail-related configuration | Low |
| DNS server version information returned | May reveal additional infrastructure details | Low |
| WordPress REST API reference exposed | Reveals the presence of a standard WordPress API endpoint | Informational |
| ModSecurity detected | Indicates an existing web traffic filtering layer | Positive security observation |
| DNSSEC reported as unsigned | DNS responses do not have DNSSEC validation protection through this domain configuration | Medium |
| Local network hosts discovered | Provides visibility into devices responding on the local network | Informational |

**Risk Assessment Notes:**

- No exploitation was performed.
- No unauthorized access was attempted.
- No open ports or vulnerable services were established by the Zenmap ping scan.
- Technology version detection alone does not confirm outdated or vulnerable software.
- The DNSSEC observation is a configuration consideration, not proof of an active attack.

---

## 6. Recommendations

1. Review domain DNS configuration and consider enabling DNSSEC where supported.
2. Keep WordPress, plugins, themes, and server software updated.
3. Review publicly exposed HTTP response headers and disclose only information necessary for normal operation.
4. Continue using a properly configured Web Application Firewall.
5. Review DNS records periodically and remove records that are no longer required.
6. Maintain secure cookie attributes such as Secure and HttpOnly where appropriate.
7. Monitor domain and DNS configuration changes.
8. Perform future security assessments only with valid written authorization and a clearly defined scope.

---

## 7. Conclusion

The Week 2 project provided practical experience with footprinting, reconnaissance, DNS enumeration, web technology identification, HTTP header analysis, WAF detection, and local network discovery.

WHOIS provided domain registration information. nslookup and DNSRecon identified DNS information and records. WhatWeb detected the website technology stack, while curl provided HTTP response headers. WAFW00F identified ModSecurity as the detected Web Application Firewall.

The Zenmap ping scan of the learner's own local network identified two live hosts out of 256 scanned IP addresses.

Overall, the exercise helped develop an understanding of how reconnaissance tools collect publicly available information and how network discovery can be performed in a controlled environment.

No confirmed exploitable vulnerability was established by the collected evidence.

---

## 8. Evidences Collected

The following screenshots should be placed inside the `Screenshots` folder.

| No. | Evidence | Suggested File Name |
|---|---|---|
| 1 | WHOIS lookup output | whois.png |
| 2 | WhatWeb output | whatweb.png |
| 3 | nslookup output | nslookup.png |
| 4 | curl HTTP headers | curl-headers.png |
| 5 | WAFW00F output | wafw00f.png |
| 6 | DNSRecon output | dnsrecon.png |
| 7 | Windows IP configuration | ipconfig.png |
| 8 | Zenmap scan result | Week2-Zenmap-Scan-Result.png |
| 9 | Zenmap network topology | Week2-Zenmap-Topology.png |

**Evidence Directory:**

```text
Week-2-Penetration-Testing-Report/
│
├── README.md
│
├── Report/
│
└── Screenshots/
    ├── whois.png
    ├── whatweb.png
    ├── nslookup.png
    ├── curl-headers.png
    ├── wafw00f.png
    ├── dnsrecon.png
    ├── ipconfig.png
    ├── Week2-Zenmap-Scan-Result.png
    └── Week2-Zenmap-Topology.png
```

**End of Report**
