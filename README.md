# Audit and Hardening Project

This repository contains the project for auditing and hardening a Linux system using **Lynis** and **CIS Benchmark** tools. The goal of this project was to improve system security by identifying vulnerabilities and applying best practices to reduce attack surfaces.

## Table of Contents
- [Introduction](#introduction)
- [Tools Used](#tools-used)
- [Environment Setup](#environment-setup)
- [How to Run Audits](#how-to-run-audits)
- [Audit Results and Reports](#audit-results-and-reports)
- [Project Summary](#project-summary)
- [Contributing](#contributing)
- [License](#license)

## Environment Setup
The environment for these vulnerability scans included the following:
- **Virtual Machine**: [Metasploitable](https://sourceforge.net/projects/metasploitable/) - an intentionally vulnerable Linux virtual machine used for testing purposes.
- **Virtualization Software**: VirtualBox was used to run the Metasploitable VM.
- **Network Configuration**: Both VMs (Kali Linux and Metasploitable) were configured in NAT mode, allowing them to communicate with each other.
