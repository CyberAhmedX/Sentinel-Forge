# DAY 18 Command and Control Introduction  
## Command and Control Introduction (High-Level)  
This document provides a high-level overview of Command and Control (C2), its importance, and common frameworks used by adversaries.  

### 1. Definition of Command and Control (C2)  
Command and Control (C2 or C&C) refers to the techniques and infrastructure that adversaries use to communicate with and control compromised systems within a target network.  

**MITRE ATT&CK Definition**: C2 consists of "techniques that adversaries may use to communicate with systems under their control within a victim's Network."  

In practice, C2 allows an attacker to gain and maintain control over a victim's computer to perform various post-exploitation actions.  

The MITRE ATT&CK framework documents 18 distinct C2 techniques (as of this guide).  

### 2. Tactical Importance for Adversaries  
Establishing a C2 channel is a critical step for an adversary to achieve their Actions on Objective (their ultimate goal).  

A stable C2 channel is required for most post-exploitation activities, including:  
- Stealing user credentials.  
- Moving laterally to other systems within the internal network.  
- Exfiltrating sensitive information (e.g., financial data, health records).  
- Executing a final payload, such as ransomware.  

### 3. Common C2 Tools and Frameworks  
A review of four common frameworks used in offensive operations and adversary emulation.  

#### A. Metasploit  
- A popular, open-source framework owned by Rapid7 and commonly included in distributions like Kali Linux.  
- It is well-known for being easy to use and includes a wide variety of exploits and auxiliaries to probe and compromise target machines.  

#### B. Cobalt Strike  
- A commercial, professional-grade adversary emulation tool developed by Fortra.  
- Due to its effectiveness, it is frequently observed in real-world compromised environments.  
- **Analyst Note**: The cybersecurity industry has developed robust detections for Cobalt Strike. Resources like The DFIR Report are valuable for learning how to detect its presence.  

#### C. Sliver  
- An open-source adversary emulation framework developed by Bishop Fox.  
- It is designed as an open-source alternative to Cobalt Strike.  
- It is easy to set up and supports multiple C2 protocols, including:  
  - mTLS  
  - HTTP / HTTPS  
  - DNS  
  - WireGuard  

#### D. Mythic  
- The primary framework selected for this 30-day challenge.  
- A C2 framework built with modern technologies: Go, Docker, Docker-Compose, and a web-based user interface.  
- **Key Feature**: Mythic allows operators to track all payloads and C2 profiles.  
- **C2 Profiles**: These profiles define exactly how agents communicate back to the Mythic server.  
- As of this guide, the Mythic framework supports 21 different types of agents.  
