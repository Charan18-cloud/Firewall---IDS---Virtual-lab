# Firewall & IDS Virtual Lab

A controlled cybersecurity lab evaluating how **pfSense** firewall controls and **Suricata** intrusion detection respond to reconnaissance activity.

## Project impact

- Built an isolated multi-VM network in VirtualBox
- Configured pfSense for routing, DHCP, segmentation, and traffic filtering
- Deployed Suricata with the Emerging Threats Open ruleset
- Simulated a full TCP SYN scan with Kali Linux and Nmap
- Correlated attack timing with IDS alerts and firewall behavior
- Documented the difference between detection and prevention

## Architecture

```text
Kali Linux (attacker)
        |
192.168.1.0/24 lab network
        |
pfSense firewall + Suricata IDS
        |
Windows target
```

| Component | Purpose |
|---|---|
| VirtualBox | Isolated virtualization environment |
| pfSense CE | Firewall, gateway, DHCP, and network segmentation |
| Suricata | Signature- and anomaly-based network detection |
| Kali Linux | Authorized attack simulation |
| Windows VM | Lab target |
| Nmap | Network reconnaissance |
| ET Open | IDS signature ruleset |

## Test scenario

The Kali VM performed a TCP SYN scan against the Windows target:

```bash
nmap -sS -p- 192.168.1.100
```

The experiment scanned all 65,535 TCP ports. pfSense filtered traffic while Suricata recorded stream anomalies during the scan window, including `SURICATA STREAM bad window update` events.

## Findings

1. **Filtered traffic can still produce useful detection evidence.**
2. **Layered controls improve visibility:** pfSense provided isolation and filtering; Suricata inspected traffic behavior.
3. **Detection is not prevention:** IDS-only mode generated alerts without automatically blocking the source.
4. **Reconnaissance leaves observable patterns** that can support investigation and tuning.

## Skills demonstrated

- Network security architecture
- Firewall and IDS administration
- Linux and Windows virtualization
- TCP/IP and packet-flow analysis
- Security testing in an authorized lab
- Log analysis and technical documentation

## Repository contents

- `Firewall/` — project materials
- `README.md` — concise technical overview
- `LICENSE` — repository license

## Next steps

- Compare IDS-only and IPS blocking modes
- Add controlled SSH brute-force and service-enumeration tests
- Write custom Suricata rules
- Forward alerts to a SIEM such as Wazuh, Splunk, or the Elastic Stack
- Benchmark detection latency and resource use

## Academic context

Developed by **Charan Teja Badavath** as graduate cybersecurity research at Arkansas State University. All activity was performed in an isolated, authorized environment; no public systems were targeted.
