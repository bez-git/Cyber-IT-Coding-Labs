# Active Directory + Sysmon Detection Lab

## Overview
This project documents the design and implementation of a **simulated enterprise Active Directory environment** integrated with **Sysmon** to generate high-fidelity Windows endpoint telemetry. 

Captures **realistic misconfigurations and troubleshooting scenarios** (DNS resolution issues, NAT routing, domain join edge cases) to mirror challenges encountered in production environments.

---

## Objectives
- Deploy and configure an Active Directory domain
- Join and authenticate a Windows endpoint using domain credentials
- Validate DNS dependency and authentication behavior
- Instrument endpoints with Sysmon for enhanced telemetry
- Generate and correlate authentication, privilege, and process execution events
- Capture security-relevant logs used in SOC investigations

---

## Environment Summary

**Platform: & Network Model ** VMware Workstation   &  NAT-isolated enterprise-style network  
**Platform:** Windows Server 2022 | Active Directory Domain Services & DNS | & Windows 11 | Domain-joined user workstation | 
---

## Active Directory Architecture
- Single domain controller providing identity, authentication, and authoritative DNS services
- Organizational Units structured to reflect enterprise separation:
  - Administrative accounts
  - Standard user accounts
  - Service and system accounts
- Multiple domain users created to generate:
  - Standard user authentication activity
  - Help desk–level access patterns
  - Administrative privilege usage
This structure enabled realistic testing of authentication flows, privilege assignment, and credential usage behavior.


<img width="500" height="400" alt="image" src="https://github.com/user-attachments/assets/4f148f35-97eb-4dd9-b625-e2e424e136e8" />
<img width="500" height="413" alt="image" src="https://github.com/user-attachments/assets/d1701e94-d6ea-45a7-a4ec-f20b498498c2" />
<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/4d4b37a9-6846-4f3e-98a6-da7320ab9262" />
<img width="300" height="50" alt="image" src="https://github.com/user-attachments/assets/dc422020-268f-4994-a14d-5aa223f805c2" />
<img width="400" height="100" alt="image" src="https://github.com/user-attachments/assets/11980305-7b80-4a60-9ec9-51f339ce1e16" />
<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/304a8ace-3268-46e9-b0ac-7c246d2a42dd" />
<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/f1e3fcfb-3126-4d88-a6d9-95bbfed8047c" />
<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/0f59d545-b282-43c4-872b-20ef6ff67f95" />
<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/c7620b00-adda-42fb-b240-dc54fb4d2ed0" />
<img width="500" height="200" alt="image" src="https://github.com/user-attachments/assets/142b0bab-f6aa-4907-b9c7-8f4bca65ab16" />
<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/c21ae72f-a7e0-4b98-a52a-970eaadff4c6" />
<img width="500" height="250" alt="image" src="https://github.com/user-attachments/assets/134328a2-f863-4fbc-ba96-506977dca3db" />
<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/82e27d9a-781b-4ab2-abf4-d01e5c9a3c96" />
<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/8c224a18-0a63-4ed4-8ea9-a6a121bc4968" />
<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/a6bce640-8eda-4b24-9eb2-d4305d26d12f" />
<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/40cf1b6e-4fe6-49ac-8a02-7e4ef7944d67" />


---

## Domain Join & Authentication Validation

The Windows 11 workstation was successfully joined to the domain and validated through:
- Domain-based authentication
- Logon server verification
- Name resolution dependency testing

Troubleshooting during this phase highlighted:
- Active Directory’s strict dependency on DNS correctness
- The impact of gateway and NAT alignment issues in virtualized environments

---
## Policy & Object Configuration
Group Policy Objects and domain-level settings were applied to introduce realistic security controls, including:
- Account and password policy enforcement
- Authentication and logon auditing
- Baseline user and system restrictions




---
## Sysmon Deployment
A hardened configuration was used to prioritize security-relevant telemetry over default verbose logging.

Deployment was validated by confirming:
-- Sysmon service status verification
- Availability of Sysmon operational logs
- Continuous event generation during user and administrative activity

### Telemetry Captured
---
<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/75b18465-0d6e-4308-a097-923abb4cd120" />
<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/9830e0be-160b-4293-807b-beecdbcbf494" />
<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/bddcf2db-d441-4501-a5d9-99a05b78ed5a" />
<img width="400" height="100" alt="image" src="https://github.com/user-attachments/assets/82337427-c408-40c2-9038-cd30e9057682" />
<img width="500" height="50" alt="image" src="https://github.com/user-attachments/assets/986ba26d-bf75-4cb5-876b-a2597ed87b03" />
<img width="400" height="400" alt="Screenshot 2025-12-17 191414" src="https://github.com/user-attachments/assets/7606f1a2-0af7-427c-b0fb-3a8af7721959" />

Author - Bez
