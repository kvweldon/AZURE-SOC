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
![image](https://github.com/kvweldon/AZURE-SOC/assets/141193154/38a0f3fe-4e0f-43ed-b5bd-f900c9977100)



## Architecture After Hardening / Security Controls
![image](https://github.com/kvweldon/AZURE-SOC/assets/141193154/d2818f6a-b52f-40cf-a891-ebbff3fb0091)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were initially deployed with unrestricted access to the internet. The Virtual Machines lacked proper Network Security Groups and built-in firewalls, leaving them vulnerable to potential threats. Additionally, all other resources were deployed with public endpoints exposed to the Internet, rendering the use of Private Endpoints unnecessary. This configuration posed significant security risks and potential avenues for unauthorized access or cyberattacks.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation. The built-in firewalls for the other resources were reconfigured and I created a Private Endpoint, which only allowed access to the resources from within the VNet.

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

In this project, I setup of a mini honeynet in Microsoft Azure and integrated log sources into a Log Analytics workspace. To bolster threat detection capabilities, I deployed Microsoft Sentinel, a cutting-edge SIEM solution, to automatically generate alerts and incidents based on the ingested logs.

To assess the impact of my security measures, I conducted an evaluation of metrics in the vulnerable environment prior to implementing security controls. After implementing the measures, I reevaluated the metrics to observe any changes in security events and incidents. The results were remarkable, with a significant reduction in security events and incidents, validating the effectiveness of my security controls.

However, it's essential to recognize that the network's resource usage patterns can influence the number of generated security events and alerts. In scenarios where regular users heavily utilize network resources, there is a possibility of observing an increased volume of security events and alerts within the 24-hour period following the implementation of security controls. To mitigate false positives, a careful analysis of these events is necessary to distinguish genuine threats from legitimate user activities.

In summary, this project showcases the successful implementation of a well-designed honeynet integrated with a robust SIEM solution to bolster threat detection and incident response capabilities. The measured reduction in security events and incidents underscores the effectiveness of the security controls applied. As a cybersecurity analyst, I understand the importance of contextually analyzing security events, taking into account network usage patterns, to ensure accurate threat identification and optimize incident response efforts.
