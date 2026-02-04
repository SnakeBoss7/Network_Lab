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
| **Kali Linux** | Kali Linux |  Attacker | Attack simulation & penetration testing |
| **Ubuntu Server** | Ubuntu |  IDS/Router | Suricata IDS, Splunk Indexer, Network routing |
| **Win-10** | Windows 10 |  Victim | Target system for attack simulations |

---

##  Network Architecture

![Network Lab Setup](NET-LAB.drawio.png)

### Network Topology

```
┌─────────────────────────────────────────────────────────────────────┐
│                         EXTERNAL NETWORK                            │
│                      (NAT - 192.168.50.0/24)                        │
└───────────────────────────────┬─────────────────────────────────────┘
                                │
                    ┌───────────▼───────────┐
                    │   Ubuntu Server       │
                    │   (Router/IDS)        │
                    │   ─────────────────   │
                    │   WAN: 192.168.50.3   │
                    │   LAN: 10.10.10.1     │
                    │   ─────────────────   │
                    │    Suricata IDS       │
                    │    Splunk Indexer     │
                    └───────────┬───────────┘
                                │
┌───────────────────────────────┼───────────────────────────────────┐
│                    INTERNAL LAN (10.10.10.0/24)                    │
│                                                                    │
│   ┌─────────────────┐                    ┌─────────────────┐      │
│   │  Kali Linux     │                    │  Windows 10     │      │
│   │  (Attacker)     │ ──────────────────▶│  (Victim)       │      │
│   │  ────────────   │     Attack Flow    │  ────────────   │      │
│   │  IP: 10.10.10.2 │                    │  IP: 10.10.10.5 │      │
│   │  GW: 10.10.10.1 │                    │  GW: 10.10.10.1 │      │
│   └─────────────────┘                    └─────────────────┘      │
│                                                                    │
└────────────────────────────────────────────────────────────────────┘
```

---

##  Network Configuration

### IP Address Assignments

| Machine | Interface | IP Address | Subnet Mask | Gateway | Network Type |
|---------|-----------|------------|-------------|---------|--------------|
| **Kali Linux** | eth0 | `10.10.10.2` | `255.255.255.0` | `10.10.10.1` | Internal LAN |
| **Ubuntu Server** | enp7s0 (LAN) | `10.10.10.1` | `255.255.255.0` | - | Internal LAN |
| **Ubuntu Server** | enp1s0 (WAN) | `192.168.50.3` | `255.255.255.0` | `192.168.50.1` | NAT Network |
| **Windows 10** | Ethernet | `10.10.10.5` | `255.255.255.0` | `10.10.10.1` | Internal LAN |







##  Attack Simulation & Detection (Atomic Red Team)

### Simulated attacks used to test and validate the detection pipeline

| Tactic ID | Tactic Name | Technique | Documentation |
|-----------|-------------|-----------|---------------|
| **TA0043** | Reconnaissance | Port Scanning | [ Port Scan](./MITRE/TA0043-Reconnaissance/PortScan.md) |
| **TA0006** | Credential Access | SSH Brute Force | [ Brute Force](./MITRE/TA0006-CredentialAccess/bruteForce.md) |
| **TA0006** | Credential Access | SMB Brute Force | [ Brute Force](./MITRE/TA0006-CredentialAccess/bruteForce.md) |
| **TA0011** | Command & Control | HTTP Beaconing | [ HTTP Beaconing](./MITRE/Command&control/http_beconing.md) |

---

