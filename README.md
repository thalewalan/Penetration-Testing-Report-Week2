# 🛡️ Penetration Testing Report: Footprinting, Reconnaissance & Network Scanning

> **W2-PM-FINAL | Cybersecurity Internship | Networkwalks**

![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-Reconnaissance%20%26%20Scanning-blue?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-Kali%20Linux%20%2F%20Windows-orange?style=for-the-badge)
![Internship](https://img.shields.io/badge/Program-Cybersecurity%20Internship-purple?style=for-the-badge)

---

## 📋 Table of Contents
1. [Liability Disclaimer](#1-liability-disclaimer)
2. [Introduction & Overview](#2-introduction--overview)
3. [Tools Used](#3-tools-used)
4. [Activities Performed](#4-activities-performed)
   - [4.1 Footprinting & External Reconnaissance](#41-footprinting--external-reconnaissance)
   - [4.2 Open Source Intelligence (OSINT) & theHarvester](#42-open-source-intelligence-osint--theharvester)
   - [4.3 Local Network Discovery & Zenmap](#43-local-network-discovery--zenmap)
5. [Risk Analysis / Impact](#5-risk-analysis--impact)
6. [Recommendations](#6-recommendations)
7. [Conclusion](#7-conclusion)

---

## 1. Liability Disclaimer

> **📌 AUTHORIZATION & LEGAL DISCLAIMER**  
> I have performed these activities only on systems & devices where I had secured written permission or devices that I own myself. All materials are for education and research purposes only. Do not use anything from here to break the law. The instructor, authors, and Networkwalks are not responsible for misuse. Unauthorized access is a crime in most jurisdictions.

---

## 2. Introduction & Overview

This repository documents the practical deliverables for **Week 2 (`W2-PM-FINAL`)** of the Cybersecurity & Ethical Hacking Internship. It covers the comprehensive footprinting of the `networkwalks.com` target domain using multiple Kali Linux reconnaissance utilities (`W2-PM1`), open-source intelligence gathering via `theHarvester`, and local subnet discovery using Zenmap (`W2-PM5`). 

This dual-phase approach demonstrates the methodology of transitioning from passive OSINT and external domain fingerprinting to active internal network topology mapping.

---

## 3. Tools Used

| Tool / Platform | Purpose & Functionality |
| :--- | :--- |
| **Kali Linux & EndeavourOS** | Operating systems utilized for executing reconnaissance and security analysis tools. |
| **WHOIS** | Queries domain registries to discover administrative contacts, registrar info, creation/expiry dates, and name servers. |
| **WhatWeb** | Fingerprints web server software, content management systems (WordPress), version numbers, and backend technologies. |
| **Nslookup** | Performs DNS queries to resolve domain names to IP addresses via designated DNS resolvers (`8.8.8.8`). |
| **Curl (-I)** | Fetches HTTP response headers to inspect server banners, cookies, caching policies, and exposed API endpoints. |
| **Wafw00f** | Identifies the presence and specific vendor signature of Web Application Firewalls (WAF). |
| **DNSRecon** | Conducts thorough DNS record enumeration including NS, MX, TXT/SPF, and SRV records. |
| **theHarvester** | Gathers emails, subdomains, and host data from public search engine modules (Baidu). |
| **Zenmap & Nmap** | Performs subnet ping scans (`-sn`) to discover live hosts, MAC addresses, and generate network topologies. |

---

## 4. Activities Performed

### 4.1 Footprinting & External Reconnaissance
Reconnaissance was performed against `networkwalks.com` using six primary Kali Linux utilities to map the target's digital footprint:

> **📌 MODULE 1 RECONNAISSANCE FINDINGS**  
> * **WHOIS:** Confirmed registrar (GoDaddy, LLC), creation date (2019-11-06), expiry (2027-11-06), and HostGator name servers.  
> * **WhatWeb:** Fingerprinted Apache web server running WordPress 7.1.4, Bootstrap 7.1.4, jQuery 3.7.1, and WordPress Download Manager 3.3.76 on IP `192.232.216.135`.  
> * **Nslookup:** Resolved `networkwalks.com` to `192.232.216.135` using Google Public DNS (`8.8.8.8`).  
> * **Curl -I:** Inspected response headers confirming HTTP/1.1 200 OK, secure `__wpdm_client` cookies, and exposed `/wp-json/` API endpoints.  
> * **Wafw00f:** Successfully detected that the site is protected by ModSecurity (SpiderLabs) WAF.  
> * **DNSRecon:** Enumerated full DNS record set including SOA, NS, MX (`mail.networkwalks.com`), A, SPF/TXT, and SRV autodiscover records.

### 4.2 Open Source Intelligence (OSINT) & theHarvester
Using `theHarvester` configured with search modules (e.g., Baidu), public infrastructure enumeration was executed against targets. This demonstrated how external auxiliary tools map associated domain assets and subdomain structures (such as `c.urs.microsoft.com`) without active probing.

### 4.3 Local Network Discovery & Zenmap
To satisfy network discovery requirements, a host discovery ping scan (`nmap -sn --unprivileged 10.0.0.0/24`) was executed across the local subnet. The scan mapped active nodes within 3.44 seconds:

> **📌 ZENMAP LOCAL SUBNET DISCOVERY SUMMARY**  
> * **10.0.0.1:** Host is up (0.00081s latency) | MAC: `52:54:00:12:35:00` (QEMU virtual NIC)  
> * **10.0.0.2:** Host is up | Active node visible in Zenmap topology view  
> * **10.0.0.3:** Host is up (0.0013s latency) | MAC: `52:54:00:12:35:00` (QEMU virtual NIC)

---

## 5. Risk Analysis / Impact

| # | Risk / Finding | Evidence / Observation | Potential Impact | Risk Level |
| :-: | :--- | :--- | :--- | :--- |
| **1** | Web technology version exposure | WhatWeb identified WordPress 7.1.4 and WP Download Manager | Attackers may check version-specific CVEs for known vulnerabilities | **Medium** |
| **2** | Server IP address identification | Nslookup resolved domain to `192.232.216.135` | Exposes the direct network hosting location of the web application | **Low** |
| **3** | HTTP technical header disclosure | Curl exposed server headers and `/wp-json/` endpoints | Aids in technology profiling and REST API enumeration | **Low** |
| **4** | WAF technology fingerprint | Wafw00f identified ModSecurity (SpiderLabs) | Reveals defensive architecture details to attackers | **Low** |
| **5** | DNS infrastructure profiling | DNSRecon mapped NS, MX, SPF, and SRV records | Provides a comprehensive overview of external mail and DNS architecture | **Medium** |
| **6** | Local network host visibility | Zenmap identified active IP/MAC nodes on LAN | Exposes connected devices on internal subnets to potential discovery | **Medium** |

---

## 6. Recommendations

> **📌 KEY SECURITY RECOMMENDATIONS**  
> 1. **Review Publicly Exposed Information:** Regularly audit what software versions and server banners are visible externally.  
> 2. **Maintain Patch Management:** Keep CMS platforms, themes, and plugins updated against recent security advisories.  
> 3. **Harden HTTP Headers:** Remove or sanitize unnecessary server banners and REST API disclosures where appropriate.  
> 4. **Monitor WAF Configuration:** Ensure ModSecurity rulesets are continuously tuned to log and block malicious probes.  
> 5. **Perform Internal Network Audits:** Regularly scan local subnets to detect unauthorized or rogue connected devices.

---

## 7. Conclusion

During Week 2 of the Cybersecurity & Ethical Hacking internship, practical activities covering footprinting, reconnaissance, and network scanning were successfully completed. The exercises demonstrated that comprehensive information gathering is a critical foundational step in security assessments. By systematically analyzing domain registries, HTTP headers, DNS configurations, and local subnet topologies, security professionals gain deep insight into target environments while adhering strictly to authorized scopes.

## 8. Evidence
All execution outputs, terminal logs, and graphical topologies have been captured and stored locally in the `Screenshots/` directory.

https://github.com/thalewalan/Penetration-Testing-Report-Week2/blob/main/screenshots/1.png
https://github.com/thalewalan/Penetration-Testing-Report-Week2/blob/main/screenshots/2.png
https://github.com/thalewalan/Penetration-Testing-Report-Week2/blob/main/screenshots/3.png
https://github.com/thalewalan/Penetration-Testing-Report-Week2/blob/main/screenshots/4.png
https://github.com/thalewalan/Penetration-Testing-Report-Week2/blob/main/screenshots/5.png
https://github.com/thalewalan/Penetration-Testing-Report-Week2/blob/main/screenshots/6.png
https://github.com/thalewalan/Penetration-Testing-Report-Week2/blob/main/screenshots/7.png
https://github.com/thalewalan/Penetration-Testing-Report-Week2/blob/main/screenshots/8.png
https://github.com/thalewalan/Penetration-Testing-Report-Week2/blob/main/screenshots/9.png
https://github.com/thalewalan/Penetration-Testing-Report-Week2/blob/main/screenshots/10.png
