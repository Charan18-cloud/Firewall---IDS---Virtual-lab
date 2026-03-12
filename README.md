# 🔐 Internet Security Solutions: Firewall & Intrusion Detection System Evaluation in a Virtual Lab Environment

**Author:** Charan Teja Badavath  
**Student ID:** 50972929  
**Program:** M.S. in Applied Digital Technology (Cloud & Cybersecurity)  
**Course:** Directed Individual Study, CRN-6093  
**Institution:** Neil Griffin College of Business, Arkansas State University  
**Faculty Advisor:** Dr. James McGinnis  
**Date:** February 27, 2026

---

## 📋 Abstract

This project evaluates the security provided by firewall rules and an Intrusion Detection System (IDS) against common internet-based attacks on a small virtual network. A controlled virtual lab was built using open-source security tools to simulate port scanning and brute-force attacks. Firewall and IDS logs were analyzed to assess detection and blocking effectiveness, demonstrating the value of a layered (defense-in-depth) security approach.

**Keywords:** Internet Security, Firewall, Intrusion Detection System, Cybersecurity, Network Security

---

## 🎯 Research Objectives

- Design a virtual network that replicates an internet-connected environment
- Deploy internet security strategies: a firewall (pfSense) and an IDS (Suricata)
- Simulate real-world internet attacks in a safe, isolated lab
- Analyze firewall logs and IDS alerts to evaluate detection and blocking capability

---

## 🛠️ Tools & Technologies

| Tool | Role |
|------|------|
| **VirtualBox** | Virtualization platform |
| **pfSense CE** | Firewall / Gateway |
| **Suricata** | Intrusion Detection System (IDS) |
| **Kali Linux** | Attacker VM |
| **Windows VM** | Target system |
| **Nmap** | Network reconnaissance / port scanner |
| **ET Open Ruleset** | IDS signature database (Emerging Threats) |

---

## 🏗️ Lab Architecture

```
[ Kali Linux - Attacker ]
         |
         | (Internal LAN: 192.168.1.0/24)
         |
[ pfSense Firewall + Suricata IDS ]
   LAN: 192.168.1.1  |  WAN: 10.0.2.15 (NAT)
         |
[ Windows VM - Target: 192.168.1.100 ]
```

Three virtual machines were deployed on VirtualBox:
- **pfSense VM** — Two interfaces (WAN via NAT, LAN at 192.168.1.1) with Suricata IDS installed
- **Kali Linux VM** — Attacker machine on the internal LAN
- **Windows VM** — Target machine with IP assigned via pfSense DHCP (192.168.1.100)

---

## ⚙️ Configuration Summary

### Firewall (pfSense)
- WAN Interface (em0): NAT — simulates internet connectivity
- LAN Interface (em1): Static IP 192.168.1.1 — internal gateway
- DHCP enabled for internal hosts (192.168.1.0/24 subnet)
- DNS: Cloudflare (1.1.1.1) + Google (8.8.8.8)

### IDS (Suricata)
- Installed via pfSense Package Manager
- Monitoring Interface: LAN (em1)
- Mode: IDS-only (blocking disabled)
- Ruleset: ET Open (Emerging Threats)
- Pattern Match Mode: AUTO

---

## 🧪 Attack Simulation

A **TCP SYN stealth scan** was executed from Kali Linux against the Windows VM:

```bash
nmap -sS -p- 192.168.1.100
```

| Parameter | Detail |
|-----------|--------|
| Scan Type | TCP SYN (half-open) stealth scan |
| Ports Scanned | All 65,535 TCP ports |
| Source | Kali Linux (internal LAN) |
| Target | Windows VM — 192.168.1.100 |

---

## 📊 Experimental Results

| Parameter | Result |
|-----------|--------|
| Attack Type | Nmap TCP SYN Scan |
| Source VM | Kali Linux (internal LAN) |
| Target | Windows VM — 192.168.1.100 |
| Scan Scope | 65,535 TCP ports |
| Port State Observed | Mostly filtered / no response |
| Firewall Role | Traffic filtering and segmentation |
| IDS Evidence | Alerts in `alerts.log` (STREAM anomalies) |
| Blocking Mode | Disabled (IDS-only) |
| Detection Type | Anomaly and signature-based patterns |

### Suricata Alert Types Detected
- `SURICATA STREAM bad window update`
- `Generic Protocol Command Decode`

Alerts were clustered around the scan window, confirming a direct correlation between Nmap execution and IDS detection events.

---

## 🔍 Key Findings

1. **Port filtering does not prevent IDS detection** — Suricata detected anomalous TCP stream behaviour even when most ports were filtered
2. **Layered security architecture works** — pfSense provided network isolation; Suricata provided behavioural inspection
3. **Detection ≠ Blocking** — Running in IDS-only mode proved that detection capability is independent of prevention capability
4. **Reconnaissance is visible** — TCP SYN scan patterns generated recognizable signatures in IDS logs

---

## 🗂️ Repository Structure

```
📁 Firewall/           → Folder
📁 screenshots/         → Lab setup and results screenshots
📁 Project/             → Full research paper 
README.md               → Project overview (this file)
```

---

## ⚠️ Limitations

- Only one attack type (TCP SYN scan) was tested
- IPS (blocking) mode was not enabled
- Default ET Open rules used — no custom rules written
- Encrypted traffic was not analyzed
- No SIEM integration for statistical alert export

---

## 🚀 Future Work

- Enable IPS mode and compare detection vs. prevention results
- Simulate additional attacks: brute-force, HTTP enumeration, SMB scanning
- Develop custom Suricata detection rules
- Integrate log export with SIEM tools (ELK Stack, Splunk, Wazuh)
- Conduct performance benchmarking (CPU load, packet inspection latency)

---

## 📚 References

- aleksibovellan. (2024). *opnsense-suricata-nmaps*. GitHub. https://github.com/aleksibovellan/opnsense-suricata-nmaps
- Dented Feels. (2024). *Developing Suricata Rules for Detecting Nmap Scans*. Medium.
- Emerging Threats Community. (2025). *Double Firewall Hopping with PfSense*. https://community.emergingthreats.net
- girly-ya. (2025). *Cybersecurity-Lab-pfSense-Wazuh-Honeypot*. GitHub.
- Netgate Documentation. (2025). *Multi-WAN and NAT*. https://docs.netgate.com/pfsense/en/latest/multiwan/nat.html
- Scarfone, K. A., & Mell, P. M. (2007). *Guide to Intrusion Detection and Prevention Systems (IDPS)* (NIST SP 800-94). NIST.
- TechTarget. (2025). *What is defense in depth?* https://www.techtarget.com/searchsecurity/definition/defense-in-depth

---

## 📄 License

This project is submitted as an academic research paper for Arkansas State University. All experiments were conducted in an isolated virtual environment. No public networks or real systems were used.
