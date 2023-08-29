# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://github.com/Manny-D/Azure-Honeynet-SOC/assets/99146530/e747ba66-1fee-43ce-9610-f4d8bf174403)

</br>

## Introduction

In this project, I built a mini honeynet in Azure and ingested log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. Initially I measured some security metrics in the insecure environment for 24 hours, then applied some security controls to harden the environment, measured metrics for another 24 hours, and documented the results below. 

</br>

The metrics I will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)
</br>

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)
</br>

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)
</br>

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
</br>

## Attack Maps Before Hardening / Security Controls
![SQL Auth Failures](https://github.com/Manny-D/Azure-Honeynet-SOC/assets/99146530/a02db775-8744-4c07-8d54-5fc7faaf8731)
![NSG Allowed Inbound Malicious Flows](https://github.com/Manny-D/Azure-Honeynet-SOC/assets/99146530/d6eb318c-f683-4b48-bcab-7c644ad29887)
![Linux Syslog Auth Failures](https://github.com/Manny-D/Azure-Honeynet-SOC/assets/99146530/7d8df50f-e641-4ab1-b393-b14446b5bee8)<br>
![Windows RDP/SMB Auth Failures](https://github.com/Manny-D/Azure-Honeynet-SOC/assets/99146530/8fa6a3fd-7a07-4adf-b409-151846a9c6a3)<br>
</br>


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
- Start Time: 8/26/2023, 1:25:15.182 PM
- Stop Time: 8/27/2023, 1:25:15.182 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 15319
| Syslog                   | 3728
| SecurityAlert            | 2
| SecurityIncident         | 175
| AzureNetworkAnalytics_CL | 2159
</br>

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```
</br>

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
- Start Time: 8/27/2023 19:30:12
- Stop Time: 8/28/2023 19:30:12

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8348
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0
</br>

## Result Metrics:</br>
<img width="693" alt="Screenshot 2023-08-28 at 8 11 20 PM" src="https://github.com/Manny-D/Azure-Honeynet-SOC/assets/99146530/7b4aa154-434b-4531-bb01-f7230a0c42e9">
</br>

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
