# Netcat Shells: Exposing Kali Linux to Windows

## 📖 Overview
This document provides a step-by-step walkthrough of establishing both a **Bind Shell** and a **Reverse Shell** using Netcat (`nc`), specifically focusing on scenarios where the Windows machine gains command-line access to the Kali Linux machine (`/bin/bash`). 

---



🛠️ End-to-End Workflow: 

Custom Payload
 Generation and Multi-Handler Execution

Understanding how payloads are structured, delivered, and handled is foundational to analyzing network vulnerabilities and defensive gaps. 💻

This weekend, I walked through a complete lab scenario focusing on custom executable payload generation using msfvenom and managing inbound connections via Metasploit’s multi/handler framework. Documenting these step-by-step processes is incredibly valuable for building consistency and technical clarity.

Here is the breakdown of the execution methodology:

1️⃣ Target Identification & Subnet Mapping: I began by mapping my local subnet utilizing ifconfig and arp-scan to confirm the network architecture and identify the active target host.
<img width="1920" height="922" alt="VirtualBox_kali linux_15_07_2026_14_19_28" src="https://github.com/user-attachments/assets/9397bf53-d316-436e-8a65-47764d5f609a" />

<img width="1920" height="922" alt="VirtualBox_kali linux_15_07_2026_14_20_17" src="https://github.com/user-attachments/assets/ab37f2d2-587e-42b3-af51-fcc119ff6fd2" />


2️⃣ Payload Engineering with MSFVenom: Using msfvenom, I crafted a custom Windows executable payload (reverse.exe). I specified a reverse TCP shell payload architecture, mapping it precisely to my local listening IP (LHOST) and designated port (LPORT).

<img width="1920" height="922" alt="VirtualBox_kali linux_15_07_2026_18_02_08" src="https://github.com/user-attachments/assets/b8afef34-e035-4728-82c1-d65e9be3504c" />


3️⃣ Simulating the Delivery Vector: To mimic a realistic file delivery mechanism within the lab environment, I spun up a lightweight local web server using Python's http.server module on port 8000. This hosted the newly compiled executable for the target client.
<img width="1920" height="922" alt="VirtualBox_kali linux_15_07_2026_18_03_22" src="https://github.com/user-attachments/assets/72af0bd3-4de8-4433-8b24-ec91d6a67076" />


4️⃣ Setting Up the Listener: Shifting back to Metasploit (msfconsole), I initialized the exploit/multi/handler module. I matched the payload profile exactly to the generated executable, verified the configuration parameters using show options, and launched the exploit listener to await incoming connections.
<img width="1920" height="922" alt="VirtualBox_kali linux_15_07_2026_18_09_33" src="https://github.com/user-attachments/assets/7e05247d-a351-4f48-9f38-fc1165b9cdcc" />
<img width="1920" height="922" alt="VirtualBox_kali linux_15_07_2026_18_10_59" src="https://github.com/user-attachments/assets/1a901b42-1333-4057-b80c-67ff3220e33b" />

<img width="1920" height="922" alt="VirtualBox_kali linux_15_07_2026_18_11_32" src="https://github.com/user-attachments/assets/8ccd60bc-13db-4241-845c-67840a3ad05e" />
 

5️⃣ Execution & Session Establishment: Once the executable was triggered on the target client machine, the connection handler successfully caught the stage. A live reverse command shell session opened seamlessly, providing full command-line access to the host environment
<img width="1920" height="922" alt="VirtualBox_kali linux_15_07_2026_18_12_55" src="https://github.com/user-attachments/assets/2a4f841a-0be9-4f52-a286-a5bd2b3055dd" />
.

Consistently practicing these steps reinforces how critical exact payload matching and port management are during live testing environments.
To the cybersecurity professionals in my network: What are your preferred lightweight methods for staging and testing local payload delivery in your home labs?
