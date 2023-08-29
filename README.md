# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://github.com/Manny-D/Azure-Honeynet-SOC/assets/99146530/ccf7a1c7-9c85-4a2e-9d32-b5cb78adfaa6)


</br>

## Introduction

In this project, I built a mini honeynet in Azure and ingested log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. Initially I measured some security metrics in the insecure environment for 24 hours, then applied some security controls to harden the environment, measured metrics for another 24 hours, and documented the results below. 

</br>

### The metrics I will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

</br>

### The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 Windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

</br>

### Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

</br>

### Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

</br>


## Attack Maps Before Hardening / Security Controls

- SecurityEvent (Windows Event Logs)
  - ![Windows RDP/SMB Auth Failures](https://github.com/Manny-D/Azure-Honeynet-SOC/assets/99146530/1f3583a9-ce1b-4870-b66a-18a1131c88ee)<br><br>

- Syslog (Linux Event Logs)
  - ![Linux Syslog Auth Failures](https://github.com/Manny-D/Azure-Honeynet-SOC/assets/99146530/0550dced-78ff-45f4-8846-c90d9aeb45c1)<br><br>

- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)
  - ![NSG Allowed Inbound Malicious Flows](https://github.com/Manny-D/Azure-Honeynet-SOC/assets/99146530/0c5eaf9e-38b3-4f38-8be9-688e50d49d1a)<br><br>

- Event / EvenLog (MS SQL Server brute force logs) 
  - ![SQL Auth Failures](https://github.com/Manny-D/Azure-Honeynet-SOC/assets/99146530/64d54819-f953-455a-a7a6-b177b6ec853a)<br><br>

</br>


## Metrics Before Hardening / Security Controls

The following table shows the metrics measured in the insecure environment for 24 hours:
- Start Time: 8/26/2023, 1:25:15.182 PM
- Stop Time: 8/27/2023, 1:25:15.182 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 15319
| Syslog                   | 3728
| SecurityAlert            | 2
| SecurityIncident         | 175
| AzureNetworkAnalytics_CL | 2159

<br>

<b>Note</b>: All resources were originally deployed / exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

</br>

## Attack Maps After Hardening / Security Controls

```All map queries returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

</br>

## Metrics After Hardening / Security Controls

The following table shows the metrics measured in the environment for another 24 hours, but after I applied security controls:
- Start Time: 8/27/2023 19:30:12
- Stop Time: 8/28/2023 19:30:12

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8348
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

<br>

<b>Note</b>: Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

</br>

## Results Metrics:</br>
<img width="693" alt="Results" src="https://github.com/Manny-D/Azure-Honeynet-SOC/assets/99146530/af6a8ea8-9526-48a7-8849-fe25feb19a6d)">

</br></br>

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
