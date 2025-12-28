# Full SOC & Network Architecture Lab


**Details** This folder contains 2 Modules of 6 labs of SOC & Network Architecture

A complete end-to-end project simulating a real-world enterprise network setup, from physical cabling to advanced threat detection.

###  [1. Sensor & Detection Engineering](./1.%20Sensor%20&%20Detection%20Engineering%20.md)
* **Focus:** Monitoring, SIEM Logs, and Catching Hackers (Red Team).
* **Lab 1: Sensor Deployment:** Virtualizing Zeek/Suricata on VMware with physical port mirroring (SPAN).
* **Lab 2: Log Forwarding Pipeline:** Normalizing and shipping raw logs to a central dashboard.
* **Lab 3: Adversary Simulation (Red Team):** Executing live attacks (SQLi, DNS Tunneling, Malware Beacons) to validate SIEM alerts.

###  [2. FW Security & Segmentation](./2.%20FW%20Security%20&%20Segmentation%20&%20Resilience%20.md)
* **Focus:** Firewall Rules, VLANs, and Disaster Recovery.
* **Lab 4: Perimeter Control:** Configuring FortiGate Firewall Policies and Address Objects (ACLs).
* **Lab 5: Network Segmentation:** Architecting 802.1Q VLANs to isolate Guest traffic from the Internal LAN.
* **Lab 6: Disaster Recovery:** Performing Out-of-Band (OOB) Management via a DMZ interface to recover a locked-down appliance.

---
**Tools Used:** FortiGate 60F, VMware, Zeek, Suricata, Linux, TP-Link Switch.
