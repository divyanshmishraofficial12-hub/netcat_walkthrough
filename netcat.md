# Basic Netcat Communication: Kali Linux & Fedora

## 📖 Overview
This document provides a step-by-step walkthrough of establishing a simple, unencrypted TCP connection between two Linux machines (Kali Linux and Fedora) using **Netcat** (`nc`). This setup demonstrates how Netcat can be used to bind to a port and create a basic two-way chat interface across a local network.

---

## 🔍 Phase 1: Network Identification

Before establishing a connection, we must verify the local IP addresses of both machines to ensure they can communicate over the network.

### 1. Kali Linux (Listener) Network Configuration
On the Kali Linux machine, we open a terminal and run the `ifconfig` command to identify its assigned IP address on the `eth0` interface.
*   **IP Address:** `192.168.1.4`

*(Screenshot showing the ifconfig output on Kali Linux)*
 <img width="1920" height="922" alt="VirtualBox_kali linux_13_07_2026_15_45_32" src="https://github.com/user-attachments/assets/7bb24b4c-a3c2-4b92-8966-b5fdda308f40" />

### 2. Fedora (Client) Network Configuration
Similarly, on the Fedora live environment, we run the `ifconfig` command to identify its IP address on the `enp0s3` interface.
*   **IP Address:** `192.168.1.9`

*(Screenshot showing the ifconfig output on Fedora)*
 <img width="1920" height="922" alt="VirtualBox_Fedora_13_07_2026_15_46_49" src="https://github.com/user-attachments/assets/aee49ea3-7248-4193-a4fd-118b82811edd" />

---

## ⚙️ Phase 2: Establishing the Connection

With both IP addresses identified, we use Netcat to open a listening port on the Kali machine and connect to it from the Fedora machine.

### 1. Setting up the Listener (Kali Linux)
On Kali Linux, we instruct Netcat to listen for incoming connections on port `4444`. 

**Command:**
bash
nc -lvp 4444

-l: Listen mode (wait for an incoming connection).
-v: Verbose output (provides connection details).
-p 4444: Specifies the local port to listen on.
(Screenshot showing Kali Linux listening on port 4444 and receiving the connection)
<img width="1920" height="922" alt="VirtualBox_kali linux_13_07_2026_15_49_43" src="https://github.com/user-attachments/assets/fb3330e9-f59d-4655-8c21-dc1086f0dc42" />

. Connecting the Client (Fedora)
On the Fedora machine, we use Netcat to initiate a connection to Kali's IP address (192.168.1.4) on the specified port (4444).
(Screenshot showing Fedora connecting to the Kali Linux listener)
<img width="1920" height="922" alt="VirtualBox_Fedora_13_07_2026_15_50_05" src="https://github.com/user-attachments/assets/643a040c-3863-4150-805a-a72716f032ff" />

💬 Phase 3: Connection Verification & Chat Interface
Once the connection is established, the verbose output on Kali Linux will read: connect to [192.168.1.4] from (UNKNOWN) [192.168.1.9].
Standard input (stdin) is now tied to the network socket on both ends. Anything typed into the terminal on one machine and submitted with the Enter key will be transmitted and displayed on the other machine's terminal. As demonstrated in the screenshots, a simple greeting message successfully passes between the two hosts, confirming a working bidirectional connection.
