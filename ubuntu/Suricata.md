# Suricata Guide

## Installation

```bash
sudo apt update
sudo apt install suricata
```

## Configuration

```bash
sudo suricata -c /etc/suricata/suricata.yaml -i eth0
```

## Update Rules

```bash
sudo suricata-update
```

## Rules Management

```bash
sudo suricata-update --list-all
sudo suricata-update --list-all --json
sudo suricata-update --list-all --json --output suricata-update.json
```

##  Suricata Service

```bash
sudo systemctl start suricata
sudo systemctl enable suricata
```

## logs 


| File                       | Purpose         |
|----------------------------|-----------------|
| /var/log/suricata/eve.json | Main SOC log    |
| /var/log/suricata/fast.log | Quick alerts    |
| /var/log/suricata/stats.log| Performance     |
