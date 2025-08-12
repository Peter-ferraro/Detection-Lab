# Detection Lab

## Objective
This Detection Lab project focuses on using a Security Information and Event Management (SIEM) system to ingest and analyze logs from various sources in a controlled environment. This project focuses on creating telemetry to detect, investigate, and respond to security events in real time. By configuring log sources, and developing custom parsing rules, This hands-on experience was designed to deepen understanding of network security, attack patterns, and defensive strategies.

### Skills Learned

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used

- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.

## Steps
I started by installing Sysmon on my windows 11 VM. Sysmon is a Windows tool that generates telemetry on system activity to help with threat detection and monitoring.

My next step was to create a cloud based VM with Ubuntu 22.04 installed, I would use this machine as my Wazuh manager. (I used DigitalOcean for my VM's, please excuse the other two, they will come up in my Automation Project.)
<img width="759" height="361" alt="Screenshot 2025-07-28 212829" src="https://github.com/user-attachments/assets/b4f897e2-025f-4be3-84c0-1f81e5cc2172" />

Now I needed to create firewall rules for this machine so that inbound traffic was only allowed from sources that were necessary.
<img width="2516" height="976" alt="Screenshot 2025-08-12 142657copy" src="https://github.com/user-attachments/assets/178e700f-1228-467a-b72f-2a882da23f5e" />

At this point it was time to install Wazuh manager on my VM, This involved using ssh to securely connect to the machine, and run the installation command. Once wazuh was installed, the next step was to configure it. So I logged into Wazuh from my Windows VM and started by adding this machine as an agent, this was done by copying the information provided in the Wazuh dashboard, and running it in Powershell.

The next step was to generate telemetry and make sure it was being ingested into Wazuh. 

To start, I needed to open the ossec.conf file on my windows machine to tell Wazuh to ingest the Sysmon logs.
