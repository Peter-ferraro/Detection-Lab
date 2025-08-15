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

<img width="524" height="128" alt="Screenshot 2025-08-12 151732" src="https://github.com/user-attachments/assets/f1c3ad3f-e39d-4cdc-8e7f-8b13a9a3caf5" />

Now, I started to receive some sysmon events in my Wazuh dashboard, so I could confirm that everything was configured correctly so far.

I downloaded Mimikatz on my windows VM next. Mimikatz is a post-exploitation tool used to extract passwords, hashes, and Kerberos tickets from memory on Windows systems. I'm going to use this to generate alerts on Wazuh.

After running Mimikatz for the first time, no alerts came through to Wazuh, so I needed to reconfigure ossec.conf on the Wazuh manager machine so that it will log all events. 

<img width="625" height="386" alt="Screenshot 2025-07-28 212552" src="https://github.com/user-attachments/assets/9b478eaa-3bbe-48b3-954c-40c938ffcfde" />

Now I needed to change some configurations in Filebeat to make sure that Wazuh is ingesting these logs. So I went to filebeat.yml and changed "archives: enabled:" to true.

<img width="664" height="446" alt="Screenshot 2025-08-07 160418" src="https://github.com/user-attachments/assets/ee144b71-e0be-431a-a604-227801b40ed7" />

I then went back to the Wazuh dashboard on my Windows machine, and created a new index for archives.

<img width="723" height="170" alt="Screenshot 2025-08-07 160010" src="https://github.com/user-attachments/assets/405ec6da-12ee-459c-a610-1979d93778e7" />

I should note at this point that the last few steps were necessary because, by default, Wazuh is not configured to show all logs in the manager, only those to trigger a rule. So I made these changes to make sure that these logs will trigger an event. That being said, at this point, I was getting events for Mimikatz in Wazuh.

The last thing I did in this project is create a rule to ensure that Mimikatz is detected regardless of the name of the file. I started by changing the name of Mimikatz.exe to Mimikowz.exe

I then created a new rule in the Wazuh dashboard to detect Mimikatz from its original file name.

<img width="742" height="155" alt="Screenshot 2025-08-07 160709" src="https://github.com/user-attachments/assets/9f66e8a2-917e-4bb3-8f3b-0ae4e7cc3ea4" />

Then I ran Mimikatz again with its new altered file name.

<img width="610" height="172" alt="Screenshot 2025-08-07 160154" src="https://github.com/user-attachments/assets/dde8357e-2bbd-4b02-ab86-260360797a2e" />

I will use this lab to create automated workflows in shuffle. You can see those steps in my SOC Automation Lab.

And it worked! I got an alert in Wazuh detecting the use of Mimikatz even with an alteration to the name.

<img width="763" height="512" alt="Screenshot 2025-08-07 160552" src="https://github.com/user-attachments/assets/ac77ccdf-fdd5-4673-b0ae-05842de2ac42" />
