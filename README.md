# Building a SOC + Honeynet in Azure
![Cloud Honeynet / SOC](https://imgur.com/CYHHbBa.png)

## Introduction

In this project, I built a mini honeynet in Azure and integrated log sources from various resources into a Log Analytics workspace. Microsoft Sentinel was then used to create attack maps, trigger alerts, and generate incidents. I measured security metrics in the unsecured environment over 24 hours, applied security controls to harden the environment, and then collected metrics for another 24 hours. The results are shown below. The metrics presented are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://imgur.com/5zqtpYC.png)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://imgur.com/SIVnaUC.png)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the “BEFORE” metrics, all resources were initially deployed and exposed to the internet. The Virtual Machines had unrestricted Network Security Groups and built-in firewalls, and all other resources were deployed with public endpoints accessible from the internet—no use of Private Endpoints.

For the “AFTER” metrics, the Network Security Groups were hardened by blocking all traffic except from my admin workstation. Additionally, all other resources were secured using both built-in firewalls and Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://imgur.com/yu3jeqQ.png)<br>
![Windows RDP/SMB Auth Failures](https://imgur.com/oMWap7M.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:<br>
Start Time 2024-09-11 12:31:29<br>
Stop Time 2024-09-12 12:31:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19498
| Syslog                   | 3031
| SecurityAlert            | 1
| SecurityIncident         | 175
| AzureNetworkAnalytics_CL | 1218

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```
<br><br>
![NSG Allowed Inbound Malicious Flows](https://imgur.com/oa2EfJH.jpg)<br>
![Linux Syslog Auth Failures](https://imgur.com/wnQj3qR.jpg)<br>
![Windows RDP/SMB Auth Failures](https://imgur.com/xKWQr4x.jpg)<br>

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:<br>
Start Time 2024-09-15 05:33:03<br>
Stop Time	2024-09-16 05:33:03

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 0 (-100%)
| Syslog                   | 12 (-99.60%)
| SecurityAlert            | 0 (-100%)
| SecurityIncident         | 0 (-100%)
| AzureNetworkAnalytics_CL | 0 (-100%)

## Conclusion

In this project, a mini honeynet was set up in Microsoft Azure, with log sources integrated into a Log Analytics workspace. Microsoft Sentinel was used to trigger alerts and generate incidents based on the ingested logs. Metrics were collected in the unsecured environment before applying security controls and again afterward. The results showed a significant reduction in security events and incidents following the implementation of the security measures, highlighting their effectiveness.

It is important to note that if the network resources had been heavily utilized by regular users, it is likely that more security events and alerts would have been generated during the 24-hour period following the implementation of the security controls.
