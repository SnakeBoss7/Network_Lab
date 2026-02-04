# Port Scan
## Description
Port Scanning is basically a reconnaissance technique used to identify open ports on a target system.
cmd
```
sudo nmap -p 1-1000 10.10.10.5
```
![Port-Scan](./images/port-scan-kali.png)

### Detection

#### Suricata


```
alert tcp any any -> $HOME_NET any (
    msg:"Potential Port Scan Detected"
    flags:S;
    flow:stateless;
    threshold:type threshold,track by_src,count 30,seconds 60;
    sid:1000001;
    rev:1;
)
```
![suricata-detecion](./images/suricata-detection.png)
#### Splunk
```
index="suricata-win11" 
| stats dc(dest_port) as Unique_ports  values(dest_port) as ports by dest_ip src_ip
| where Unique_ports > 30
```
![Splunk-detection](./images/splunk-port.png)