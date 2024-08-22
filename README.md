# Building a SOC + Honeynet in Azure (Live Traffic)

![GITHUB11](https://github.com/user-attachments/assets/3bdae00c-e96d-4244-b42f-65a5330584c0)

## Introduction

In this project, I developed a mini honeynet within Azure, integrating log sources from various resources into a Log Analytics workspace. This setup was utilized by Microsoft Sentinel to generate attack maps, trigger alerts, and manage incidents. Initially, I measured security metrics in an unsecured environment for 24 hours. After implementing security controls to harden the environment, I measured the metrics again for another 24 hours. Below are the key metrics:


- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls

![github2](https://github.com/user-attachments/assets/5854a52d-be09-42b1-a4db-ac35f1a6d641)

## Architecture After Hardening / Security Controls

![github3](https://github.com/user-attachments/assets/6ad56273-b8aa-45d3-8b54-2b57025ed998)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

`For the "Before Hardening" Configuration:`

All resources were deliberately exposed to the internet to simulate an vulnerable environment. Virtual Machines were configured with open Network Security Groups and disabled built-in firewalls. Other Azure resources were deployed with public endpoints, making them directly accessible from the internet without the protection of Private Endpoints..

`For the "After Hardening" Configuration:`

Network Security Groups were configured to block all inbound traffic, with the sole exception of connections from a designated admin workstation. Additionally, we leveraged the built-in firewalls of each resource and implemented Private Endpoints, effectively isolating the resources from direct internet exposure and significantly reducing the attack surface.

## Attack Maps Before Hardening / Security Controls

![NSG Allowed Inbound Malicious Flows](https://github.com/user-attachments/assets/42116407-051d-4bf9-a61e-d23d97266278)<br>
![Linux Syslog Auth Failures](https://github.com/user-attachments/assets/d3c03b6f-5b3e-48f7-abc3-31b17febeae4)<br>
![Windows RDP/SMB Auth Failures](https://github.com/user-attachments/assets/57c45470-513e-48dc-a3ac-e1508ba13268)<br>
![MSSQL Auth Failures](https://github.com/user-attachments/assets/9a8fbce9-21ac-4085-adc1-4e99b77bc933)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
<br />
`Start Time: 2024-08-01 17:59:36` <br/>
`Stop Time: 2024-08-02 17:59:36`

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 82508
| Syslog                   | 10345
| SecurityAlert            | 4
| SecurityIncident         | 271
| AzureNetworkAnalytics_CL | 490

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
<br />
`Start Time: 2024-08-20 19:02:04` <br/>
`Stop Time: 2024-08-21 19:02:04`

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8976
| Syslog                   | 23
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

### Metric Comparison  

| Metric                                       | Change After Security Environment
| -------------------------------------------- | ---------------------------------
| Security Events (Windows VMs)                | -87.05%
| Syslog (Linux VMs)                           | -99.99%
| SecurityAlert (Microsoft Defender for Cloud) | -100.00%
| SecurityIncident (Sentinel)                  | -100.00%
| NSG Inbound Malicious Flows Allowed          | -100.00%

## Conclusion

In this project, a mini honeynet was deployed in Microsoft Azure, with log sources integrated into a Log Analytics workspace. Microsoft Sentinel was utilized to generate alerts and incidents based on these logs. Initially, metrics were captured in the insecure environment, highlighting vulnerabilities. After implementing security controls, a significant reduction in security events and incidents was observed, demonstrating the effectiveness of these measures.

It's important to note that if the network resources were more actively used by regular users, additional security events and alerts might have been generated within the 24-hour period following the implementation of security measures. This underscores the importance of continuous monitoring and adaptive security strategies in dynamic environments.
