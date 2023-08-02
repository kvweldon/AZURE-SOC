# Building a SOC + Honeynet in Azure (Live Traffic)
![image](https://github.com/kvweldon/AZURE-SOC/assets/141193154/32724fd5-3bfe-4f03-aeaa-516b85cbaa01)



## Introduction

In this project, I demonstrate my working knowledge of the Azure cloud platform. First, I build a mini honeynet within Azure and ingest log sources from various resources, including two Virtual Machines, a Storage Account and a Key Vault, into a Log Analytics workspace. This is used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. To assess the security posture, I measure pertinent security metrics within the vulnerable environment for a 24 hour duration. I then implent security controls to harden the environment, measure the same metrics for another 24 hours, then show the results below. The metrics I will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![(before)nsg-malicious-allowed-in](https://github.com/kvweldon/AZURE-SOC/assets/141193154/48739ffd-b9f0-4184-a219-77abffd239ef)
<br>
![(before)syslog-ssh-auth-fail](https://github.com/kvweldon/AZURE-SOC/assets/141193154/9a27871d-95b5-4067-9045-da3587d5f46d)
<br>
![(before)windows-rdp-smb-auth-fail](https://github.com/kvweldon/AZURE-SOC/assets/141193154/b6122e0f-fc0d-4b12-99d4-089df41a14d5)
<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-07-25 21:47
Stop Time 2023-07-26 21:47

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 175743
| Syslog                   | 5492
| SecurityAlert            | 9
| SecurityIncident         | 231
| AzureNetworkAnalytics_CL | 2463

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-07-29 11:50
Stop Time	2023-07-30 11:50

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9224
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
