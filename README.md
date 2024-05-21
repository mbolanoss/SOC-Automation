# SOC Automation

## Objective

The SOC Automation Lab project aims to create a controlled environment for detecting and responding to both real and simulated cyber attacks. This project focuses on ingesting and analyzing logs within a SIEM system ([Wazuh](https://wazuh.com/)) to use a SOAR platform ([Shuffle](https://shuffler.io/)) for establishing custom workflows that create automatic responses to specific security events. Additionally, a case manager ([TheHive](https://thehive-project.org/)) is utilized to generate and manage alerts corresponding to those produced by the SIEM system.

### Skills Learned

- Advanced understanding of SIEM concepts and practical applications.
- Creation of automatic responses to security events using a SOAR platform.
- Alert creation and management using a case manager.
- Integration of complex security tools to create a cohesive and automated security operations workflow.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used

- Wazuh as the SIEM system for log ingestion and analysis.
- Shuffle as the SOAR platform for creating automatic responses.
- TheHive as the case manager to create formal alerts for security events.
- DigitalOcean serves as the cloud provider for hosting the Wazuh manager, TheHive platform, and an Ubuntu machine.

## Steps
First of all, this is the network diagram that represents the environment developed in this project:

![Diagrama](https://github.com/mbolanoss/SOC-Automation/assets/53063734/7806b4a3-8485-4792-bfde-b4839ad38eae)

*Ref 1: Network Diagram*

The project consists in 9 steps that consist in the following:
1. Two Wazuh agents, installed on a Windows 10 machine and an Ubuntu machine, respectively, will transmit events to the Wazuh manager hosted in the cloud. These events include Mimikatz execution for the Windows 10 machine and failed login attempts, which are performed by real-life attackers, for the Ubuntu machine.
2. The Wazuh manager receives and manages these events within its dashboard.
3. The Wazuh manager forwards these events as alerts to Shuffle, which serves as input for two custom workflows resulting in automatic responses.
4. [VirusTotal](https://www.virustotal.com/gui/home/upload)  is employed to enrich the Indicators of Compromise (IOCs) associated with each event.
5. The alerts are then sent to TheHive to formally create alerts within that platform.
6. Details of the alert are transmitted by Shuffle to the SOC analyst via email.
7. The SOC analyst reviews the received emails and determines whether to initiate the automatic response.
8. If deemed necessary, the SOC analyst triggers the automatic response, which is communicated to the Wazuh manager.
9. The Wazuh manager then sends corresponding instructions to the agent to execute the automatic response for each security event.

Now, here are some screenshots of the process and the obtained results:
### Wazuh setup
![agent creation](https://github.com/mbolanoss/SOC-Automation/assets/53063734/d8051399-13d2-4c31-a965-559fab10bd14)
*Ref 2: Wazuh manager is configured and two agents are registered*

### Scenario A: Mimikatz execution in a Windows 10 machine
### Scenario B: Failed login attempts

