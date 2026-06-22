Passive Adaptive Intrusion Detection System (AIDS) – FYP2

## Project Overview
This repository contains the Passive Adaptive Intrusion Detection System (AIDS), designed to monitor Layer 2 network traffic in wired LAN environments. The system detects ARP spoofing and ICMP reconnaissance attacks in real time using Suricata and Python scripts, while maintaining normal network operations.

## Directory Structure

Adaptive_IDS_FYP2/
│
|
├── scripts/
│   ├── arp_detector.py
│   ├── suricata_alert_reader.py
│   ├── any_other_custom_script.py
│   
├── config/
│   ├── suricata.yaml
│   └── sample_eve.json
├── logs/
    ├── detected_alerts_sample.log
    ├── suspicious_mac_sample.txt
    ├── blocked_ips.txt
    └── response_actions_sample.log

## Setup Instructions

### 1. Ubuntu Monitoring Host
- OS: Ubuntu Server 22.04 LTS
- Dual NIC: 
  - `ens33` for management
  - `ens37` for mirrored traffic from the switch
- Update system packages:

sudo apt update && sudo apt upgrade -y
- Install dependencies:

sudo apt install python3-pip python3-venv tcpdump build-essential libpcap-dev -y
- Create virtual environment:

python3 -m venv ~/myenv
source ~/myenv/bin/activate
pip install --upgrade pip
pip install scapy logging

### 2. Suricata IDS
- Install Suricata (version 7.0+ recommended):

sudo apt install suricata -y
suricata -V
- Configure `suricata.yaml` to bind to mirrored interface (`ens37`).
- Run Suricata:

sudo suricata -c /etc/suricata/suricata.yaml -i ens37

### 3. Python Scripts
- `arp_detector.py` – monitors ARP traffic and logs anomalies.
- `suricata_alert_reader.py` – monitors ICMP ping traffic
- Run scripts inside virtual environment:

sudo ./myenv/bin/python3 arp_detector.py
sudo python3 -u suricata_alert_reader.py

### 4. Kali Linux Attacker
- OS: Kali Linux 2025.2
- Install arpspoof utilities:

sudo apt update && sudo apt upgrade -y
sudo apt install dsniff -y
- Ensure attacker NIC is on the same VLAN as victim.

### 5. Windows Victim
- OS: Windows 10
- Static IP configuration (e.g., 192.168.1.20)
- Same LAN segment as Kali and Ubuntu hosts.

### 6. Managed Switch
- Model: Extreme Summit X450e
- Configure SPAN (mirror) port to forward traffic to Ubuntu monitoring host.
- Use "show mirror" command to check mirror status

## Sample Logs
- `detected_alerts_sample.log` – logged anomalous events.
- `suspicious_mac_sample.txt` – tracks unknown or mismatched MAC addresses.
- `response_actions_sample.log` – records actions taken by Python scripts (logical isolation).
- 'blocked_ips.txt' - tracks IP Address from anomalous events

## Screenshots
- `ubuntu_terminal.png` – Python scripts running.
<img width="1600" height="713" alt="image" src="https://github.com/user-attachments/assets/0e1f733d-ff24-4c7a-ba63-afd42caeafff" />

- `kali_attack_simulation.png` – ARP spoofing attack.
<img width="709" height="102" alt="image" src="https://github.com/user-attachments/assets/27d80e6a-a4ac-4760-a3e4-db85a78877e9" />

- `arp_detected.png` – ARP anomaly logged.
<img width="1378" height="575" alt="image" src="https://github.com/user-attachments/assets/e71db3ee-685e-4d59-a326-7e14cf7d3415" />

- `icmp_detected.png` – ICMP ping detected.
<img width="1600" height="480" alt="image" src="https://github.com/user-attachments/assets/8024bdf9-ea9d-4391-aa05-b88ae42fe9ba" />




