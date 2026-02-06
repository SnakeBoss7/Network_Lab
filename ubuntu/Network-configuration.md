# Ubuntu Router Setup Guide

This guide configures an Ubuntu server to act as a router, providing internet access to a client machine (e.g., Windows 11) on an internal LAN.

## Step 1: Network Interface Configuration

Ensure the Ubuntu server has two Network Interfaces (NICs):

1.  **WAN Interface (Internet):** Connected to the host via NAT/Bridged (e.g., `libvirt` NAT).
2.  **LAN Interface (Internal):** Connected to the internal network (LAN) to serve the "victim" or client machine.

*Tip: Check your interface names using `ip a` (e.g., `enp1s0`, `enp7s0`).*

## Step 2: Configure Netplan (Mandatory)

Configure the network interfaces with static IP for the LAN and DHCP for the WAN.

**1. Edit the Netplan configuration:**
```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

**2. Configure the network:**
Use the structure below. Ensure indentation is correct (YAML is sensitive to spaces).
*   `enp1s0`: WAN/Internet interface (DHCP).
*   `enp7s0`: LAN/Internal interface (Static IP).

```yaml
network:
  version: 2
  ethernets:
    enp1s0:
      dhcp4: true
    enp7s0:
      dhcp4: false
      addresses:
        - 192.168.50.1/24
```

**3. Apply changes:**
```bash
sudo netplan apply
```

**4. Verify configuration:**
```bash
ip a
```
*You should see `enp7s0` is UP with IP `192.168.50.1/24`.*

## Step 3: Enable IP Forwarding

This allows the Ubuntu kernel to forward packets between the two interfaces.

**1. Enable temporarily:**
```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

**2. Verify status:**
```bash
cat /proc/sys/net/ipv4/ip_forward
```
*Output should be `1`.*

**3. Make it permanent:**
Edit the sysctl configuration:
```bash
sudo nano /etc/sysctl.conf
```
Add or uncomment the following line:
```ini
net.ipv4.ip_forward=1
```
Save and apply changes:
```bash
sudo sysctl -p
```

## Step 4: Configure NAT (Masquerading)

Enable Network Address Translation (NAT) to allow the LAN client to access the internet through the Ubuntu server.

*Replace `enp1s0` with your **WAN/Internet** interface.*
*Replace `enp7s0` with your **LAN/Internal** interface.*

```bash
# Mask outgoing traffic as coming from the WAN interface
sudo iptables -t nat -A POSTROUTING -o enp1s0 -j MASQUERADE

# Allow forwarding from LAN to WAN
sudo iptables -A FORWARD -i enp7s0 -o enp1s0 -j ACCEPT

# Allow return traffic for established connections
sudo iptables -A FORWARD -i enp1s0 -o enp7s0 -m state --state RELATED,ESTABLISHED -j ACCEPT
```