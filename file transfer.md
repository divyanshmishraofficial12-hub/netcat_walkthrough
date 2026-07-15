# Netcat File Transfer: Kali Linux & Windows

## 📖 Overview
This document provides a step-by-step walkthrough of establishing a TCP connection between a Kali Linux machine and a Windows system using **Netcat** (`nc`). This setup demonstrates how to bypass the local Windows firewall and use Netcat to host and transfer a text file over a local network.

---

## 🔍 Phase 1: Network Discovery & Configuration

Before attempting a connection, we must identify the IP addresses of both machines and ensure the network allows traffic to pass through.

### 1. Kali Linux (Listener) Network Configuration
On the Kali Linux machine, we open a terminal and run the `ifconfig` command to identify its assigned IP address on the `eth0` interface.
*   **IP Address:** `192.168.1.4`

*(Screenshot showing the ifconfig output on Kali Linux)*
![Kali Linux IP Configuration](IMG-20260715-WA0000.jpg)

### 2. Windows (Client) Firewall & IP Configuration
To ensure our incoming and outgoing Netcat connections are not blocked, we disable the Windows firewall for all profiles using an elevated command prompt. Then, we run `ipconfig` to identify the Windows machine's IP address.
*   **Command:** `netsh advfirewall set allprofiles state off`
*   **IP Address:** `192.168.1.11`

*(Screenshot showing the Windows firewall being disabled and the ipconfig output)*
![Windows Firewall and IP Configuration](IMG-20260715-WA0001.jpg)

### 3. Host Discovery Verification
Back on Kali Linux, we use `arp-scan -l` to broadcast ARP requests and verify that the Windows machine (`192.168.1.11`) is visible and reachable on the local subnet.

*(Screenshot showing the arp-scan execution and host discovery)*
![ARP Scan Discovery](IMG-20260715-WA0003.jpg)

---

## 📝 Phase 2: Payload Preparation

Next, we prepare the file or message that we want to transfer across the connection.

### 1. Creating the Target File
On Kali Linux, we use a text editor (`nano`) to create a simple text file named `prac.txt`. We then use `cat` to verify its contents.
*   **Contents:** "this is for connection"

*(Screenshot showing the creation and verification of the prac.txt file)*
![File Creation on Kali](IMG-20260715-WA0002.jpg)

---

## ⚙️ Phase 3: Execution & File Transfer

With the network configured and the file ready, we use Netcat to host the file on Kali and retrieve it from Windows.

### 1. Preparing the Windows Client Environment
On the Windows machine, we navigate to the directory where the Windows version of Netcat (`nc.exe`) is located (e.g., `C:\Users\boss\Desktop\nc.exe-master`). We run the `dir` command to confirm the executable is present.

*(Screenshot showing the Windows directory containing nc.exe)*
![Windows Netcat Directory](IMG-20260715-WA0005.jpg)

### 2. Setting up the Listener (Kali Linux)
On Kali Linux, we instruct Netcat to listen on port `4455` and redirect the contents of `prac.txt` into the connection.
*   **Command:** `nc -v -l -p 4455 < prac.txt`
    *   **`-v`**: Verbose mode.
    *   **`-l`**: Listen mode.
    *   **`-p 4455`**: Listen on port 4455.
    *   **`< prac.txt`**: Redirect the file's contents into the outbound stream.

*(Screenshot showing Kali Linux listening on port 4455 and pushing the file)*
![Kali Linux Netcat Listener](IMG-20260715-WA0004.jpg)

### 3. Connecting and Receiving Data (Windows)
On the Windows machine, we use `nc.exe` to connect to the Kali listener.
*   **Command:** `nc.exe -v 192.168.1.4 4455`

As soon as the connection is established, Kali Linux pushes the contents of `prac.txt` over the socket. The text "this is for connection" successfully appears in the Windows command prompt, verifying the cross-platform data transfer.

*(Screenshot showing Windows connecting to Kali and receiving the text payload)*
![Windows Netcat Client Connection](IMG-20260715-WA0006.jpg)
