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
*Ref 2: Wazuh manager is configured and two agents are registered.*

### Scenario A: Mimikatz execution in a Windows 10 machine

![Mimikatz exec](https://github.com/mbolanoss/SOC-Automation/assets/53063734/cb7a37eb-764a-496b-9c54-d4011da72d3b)

![Mimikatz exec with diff name](https://github.com/mbolanoss/SOC-Automation/assets/53063734/c6132401-19fb-4d16-ac89-aff3fe5b3848)

*Ref 3: An execution of Mimikatz is performed, using it's original file name and a different one, to generate and send that event to Wazuh.*

![Wazuh mimikatz search](https://github.com/mbolanoss/SOC-Automation/assets/53063734/d808bb7f-cb5d-434e-b17c-658b7012b362)

*Ref 4: The Mimikatz security event displayed in Wazuh's dashboard.*

![Initial workflow](https://github.com/mbolanoss/SOC-Automation/assets/53063734/fcfcb167-59ea-4ab6-af03-74b9881b65f7)

![Shuffle initial execution](https://github.com/mbolanoss/SOC-Automation/assets/53063734/b5522fda-6fad-457f-97b9-3479ab715d7f)

*Ref 5: In Shuffle, the workflow is created and the sending of the alert from Wazuh is verified*

![Shuffle regex node](https://github.com/mbolanoss/SOC-Automation/assets/53063734/24a1de0a-88b7-44b3-b90d-4f63ee2afba7)

*Ref 6: A regex node is added to parse the Mimikatz file SHA256 hash.*

![Added virustotal node](https://github.com/mbolanoss/SOC-Automation/assets/53063734/7a6cdcc8-5904-418a-b00d-292f7a3c12fd)

![virustotal answer](https://github.com/mbolanoss/SOC-Automation/assets/53063734/94dedd30-29d5-42db-87df-fb1ce773f5a6)

*Ref 7: A VirusTotal node is added to search for information and reports using the SHA256 hash of the Mimikatz file.*

![Added thehive node](https://github.com/mbolanoss/SOC-Automation/assets/53063734/132ce0e3-5343-4f98-b23e-7441f29616c0)

*Ref 8: TheHive node added to create an alert.*

![Alert on thehive](https://github.com/mbolanoss/SOC-Automation/assets/53063734/b2250e36-97d3-4624-aad9-446b6d91a3cc)

![Alert on thehive - detail](https://github.com/mbolanoss/SOC-Automation/assets/53063734/15e82fcf-413c-4431-abf0-991e86187304)

*Ref 9: Finally, the alert is created automatically in TheHive.*

### Scenario B: Failed login attempts

![ssh bruteforce attempts](https://github.com/mbolanoss/SOC-Automation/assets/53063734/4ee43b91-9d1f-4979-8166-ef9be0938b5b)

*Ref 10: Real-life attackers are trying to login using brute force attacks.*

![Virustotal workflow](https://github.com/mbolanoss/SOC-Automation/assets/53063734/1e0449ca-cdfb-42c1-bbe8-715b6909571d)

*Ref 11: A regex node is added to parse the source IP of the attack, and then is used to search for information and reports in VirusTotal.*

![Thehive workflow](https://github.com/mbolanoss/SOC-Automation/assets/53063734/be24869a-e635-42a6-b001-e6f2cf2c4d2b)

![Alert created in thehive](https://github.com/mbolanoss/SOC-Automation/assets/53063734/df5ebe8a-44b0-4efe-983b-9f50ff4881be)

*Ref 12: TheHive node is added and alerts are being created automatically.*

![Wazuh workflow without user interaction](https://github.com/mbolanoss/SOC-Automation/assets/53063734/b2a31645-979c-43cd-a887-5ed255b07026)

![Wazuh workflow without user interaction - result](https://github.com/mbolanoss/SOC-Automation/assets/53063734/0397b4cc-007a-43a5-871f-57901f359fd3)

*Ref 13: For test purposes, the Google DNS IP is initially used to simulate an attack and block that IP. Additionally, in the workflow, a Wazuh node is added to send a custom command to execute the automated response.*

![Complete workflow](https://github.com/mbolanoss/SOC-Automation/assets/53063734/84add2e3-ea8e-49fd-a391-ef6612c4da70)

*Ref 14: SOC analyst interaction via email is added. This is the final workflow.*

![User interaction email](https://github.com/mbolanoss/SOC-Automation/assets/53063734/822942ce-80a1-4d15-a215-7846c346777a)

*Ref 15: Sample of the email sent to the SOC analyst for them to decide whether to block the IP or not.*

![User interaction result](https://github.com/mbolanoss/SOC-Automation/assets/53063734/6d825e12-3ed7-4f74-bc83-54d1107b53db)

![Wazuh final result](https://github.com/mbolanoss/SOC-Automation/assets/53063734/480b62dd-625c-4a41-b88e-651c70a85457)

*Ref 16: The final result of blocking the IP of a real-life attacker who attempted to log in to the Ubuntu machine.*
















