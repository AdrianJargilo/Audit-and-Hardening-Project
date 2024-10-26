# Audit and Hardening Project

This repository contains the project for auditing and hardening a Linux system using **Lynis** and **CIS Benchmark** tools. The goal of this project was to improve system security by identifying vulnerabilities and applying best practices to reduce attack surfaces.

## Table of Contents
- [Introduction](#introduction)
- [Tools Used](#tools-used)
- [Environment Setup](#environment-setup)
- [Experiments and Hardening Steps](#experiments-and-hardening-steps)
  - [Exercise 1: Determining Attack Surface](#exercise-1-determining-attack-surface)
  - [Exercise 2: Auditing with Lynis](#exercise-2-auditing-with-lynis)
  - [Exercise 3: Implementing CIS Benchmark Recommendations](#exercise-3-implementing-cis-benchmark-recommendations)
- [Audit Results and Reports](#audit-results-and-reports)
- [Contributing](#contributing)
- [License](#license)

## Introduction
In this project, I performed vulnerability audits on a Kali Linux system using the following tools:
- **Lynis**: An open-source auditing tool for Unix-based systems.
- **CIS Benchmark**: A framework for system hardening developed by the Center for Internet Security (CIS).

The audit was conducted in several stages, including an initial audit using Lynis, implementing the recommended changes, and verifying improvements using a second audit. Additionally, the CIS recommendations were applied to further harden the system.

## Tools Used
- **Lynis**: Auditing and compliance tool for Linux and Unix systems.
- **CIS Benchmark**: A set of guidelines for securing system configurations.
- **Kali Linux**: The platform used for testing and implementing the security audits.

## Environment Setup
The audits were conducted in a virtual environment using **VirtualBox** with **Kali Linux** as the target system. The network was configured to ensure the system was isolated from external networks to avoid any unintended security breaches during the audit.

### Steps to Set Up the Environment
1. **Install VirtualBox and Kali Linux**:
   - Download and install VirtualBox from [https://www.virtualbox.org/](https://www.virtualbox.org/).
   - Download the Kali Linux ISO from [https://www.kali.org/downloads/](https://www.kali.org/downloads/).
   - Create a new virtual machine in VirtualBox and install Kali Linux.

2. **Install Lynis**:
   - Once Kali Linux is set up, use the following command to install Lynis:
     ```bash
     sudo apt-get update
     sudo apt-get install lynis
     ```

3. **Scripts in the Repository**:
   - To facilitate the audits, I created some scripts that automate parts of the process. You can find them in the `scripts/` directory of this repository.
   - Use these scripts to replicate the audit and hardening steps.

## Experiments and Hardening Steps

### Exercise 1: Determining Attack Surface
The first step in the project was to determine the system's attack surface by analyzing active services and network configuration. Four services were enabled on the Kali Linux system: **SSH**, **NTPsec**, **Apache**, and **PostgreSQL**.

1. **Port Scanning with `netstat` and `nmap`**:
   - Used `netstat` to identify active ports and services:
     ```bash
     sudo netstat -ntlup
     ```
     ![image](https://github.com/user-attachments/assets/37f471fa-e05a-4ada-ab05-68477f2539c1)

     This command lists all the active connections, listening ports, and the processes associated with them.
   - Verified the results with `nmap` to cross-check the open ports:
     ```bash
     nmap localhost
     ```
     The scans indicated the presence of open ports for **SSH (22)**, **HTTP (80)**, and **PostgreSQL (5432)**, as well as the **NTPsec (123)** UDP port.

2. **Firewall Configuration**:
   Initially, the firewall (`iptables`) did not contain any specific rules. I applied various rules to secure access to the active services:
   - **SSH (port 22)**: Restricted access to specific IP addresses to prevent unauthorized remote access.
     ```bash
     iptables -A INPUT -p tcp --dport 22 -s 192.168.1.100 -j ACCEPT
     iptables -A INPUT -p tcp --dport 22 -j DROP
     ```
   - **HTTP (port 80)**: Limited the rate of incoming connections to mitigate DDoS attacks.
     ```bash
     iptables -A INPUT -p tcp --dport 80 -m limit --limit 25/minute --limit-burst 100 -j ACCEPT
     iptables -A INPUT -p tcp --dport 80 -j DROP
     ```
   - **PostgreSQL (port 5432)**: Restricted access to the local network only.
     ```bash
     iptables -A INPUT -p tcp --dport 5432 -s 192.168.1.0/24 -j ACCEPT
     iptables -A INPUT -p tcp --dport 5432 -j DROP
     ```

3. **Password Policy**:
   Updated the password policy by modifying the `/etc/pam.d/common-password` configuration file to enforce:
   - Minimum password length of **8 characters**.
   - Use of uppercase and lowercase characters, numbers, and special symbols.

4. **Automatic Updates**:
   Installed `unattended-upgrades` to enable automatic security updates for the system:
   ```bash
   sudo apt-get install unattended-upgrades
   
5. **Root Login Configuration**: Disabled root login via SSH to prevent unauthorized access.
   - Edited the `/etc/ssh/sshd_config` file and set:
     ```bash
     PermitRootLogin no
     ```

6. **Kernel Configuration**: Reviewed kernel settings using `sysctl` and adjusted critical parameters in `/etc/sysctl.conf` to improve system security, including:
   - Disabled ICMP redirects to prevent **Man-In-The-Middle (MITM)** attacks:
     ```bash
     net.ipv4.conf.all.send_redirects = 0
     net.ipv4.conf.default.send_redirects = 0
     ```


