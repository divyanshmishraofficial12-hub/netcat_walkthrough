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
 
### 2. Fedora (Client) Network Configuration
Similarly, on the Fedora live environment, we run the `ifconfig` command to identify its IP address on the `enp0s3` interface.
*   **IP Address:** `192.168.1.9`

*(Screenshot showing the ifconfig output on Fedora)*
![Fedora IP Configuration](1000111128.jpg)

---

## ⚙️ Phase 2: Establishing the Connection

With both IP addresses identified, we use Netcat to open a listening port on the Kali machine and connect to it from the Fedora machine.

### 1. Setting up the Listener (Kali Linux)
On Kali Linux, we instruct Netcat to listen for incoming connections on port `4444`. 

**Command:**
```bash
nc -lvp 4444
