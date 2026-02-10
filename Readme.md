#  Network Security Lab

> A comprehensive home lab environment for security testing, attack simulation, and detection engineering using **Suricata**, **Splunk**, **Zeek**, and **MITRE ATT&CK** framework.

---



##  Lab Overview

### Host Machine
| Component | Details |
|-----------|---------|
| **OS** | Windows 11 |
| **Role** | Hypervisor Host |

### Virtual Machines

| VM | OS | Role | Purpose |
|----|-----|------|---------|
| **Kali Linux** | Kali Linux |  Attacker | Attack simulation |
| **Ubuntu Server** | Ubuntu |  IDS/Router | Suricata IDS, Splunk Indexer, Network routing |
| **Win-10** | Windows 10 |  Victim | Target system for attack simulation |

---



##  Network Configuration


![network_config](./images/network_config.drawio%20(1).png)
---

### IP Address Assignments

| Machine | Interface | IP Address | Subnet Mask | Gateway | Network Type |
|---------|-----------|------------|-------------|---------|--------------|
| **Kali Linux** | eth0 | `10.10.10.2` | `255.255.255.0` | `10.10.10.1` | Internal LAN |
| **Ubuntu Server** | enp7s0 (LAN) | `10.10.10.1` | `255.255.255.0` | - | Internal LAN |
| **Ubuntu Server** | enp1s0 (WAN) | `192.168.50.3` | `255.255.255.0` | `192.168.50.1` | NAT Network |
| **Windows 10** | Ethernet | `10.10.10.5` | `255.255.255.0` | `10.10.10.1` | Internal LAN |







##  Attack Simulation & Detection (Atomic Red Team)

### Simulated attacks used to test and validate the detection pipeline

| Tactic ID | Tactic Name | Technique | Documentation | Detection Tools |
|-----------|-------------|-----------|---------------|---------------|
| **TA0043** | Reconnaissance | Port Scanning | [ Port Scan](./MITRE/TA0043-Reconnaissance/PortScan.md) | Suricata(Signature based), Zeek(Behavioral based) |
| **TA0006** | Credential Access | SSH Brute Force | [ Brute Force](./MITRE/TA0006-CredentialAccess/bruteForce.md) | Suricata, Splunk + Linux auth.log |
| **TA0006** | Credential Access | SMB Brute Force | [ Brute Force](./MITRE/TA0006-CredentialAccess/bruteForce.md) | Suricata, Splunk + WinEventLog |
| **TA0011** | Command & Control | HTTP Beaconing | [ HTTP Beaconing](./MITRE/Command&control/http_beconing.md) | Suricata, Splunk, Zeek |

---

