# DAY 20: Mythic Server Setup Tutorial  
This guide details the hands-on steps for deploying a new cloud server, installing the Mythic C2 framework, and performing the initial security hardening.  

## 1. Vultr VM Deployment (Mythic Server)  
**Deploy New Server:**  
- Server Type: Cloud Compute, Shared CPU.  
- Location: Select desired region (e.g., Toronto).  
- Operating System: Ubuntu 22.04 LTS.  
- Server Size: Select a plan with at least 4 GB of RAM.  
- Additional Options: Disable Auto Backups and IPv6.  
- Server Hostname: Set a new hostname (e.g., your-own-mythic-handle).  
- **Action:** Deploy the new server.  

## 2. Server Access and Initial Setup  
**Access:** Use SSH to connect to the server's Public IP address as `root`.  
**Update System:** Run system update and upgrade commands:  
```bash
apt-get update
apt-get upgrade -y
```

## 3. Mythic Prerequisite Installation  
**Install Docker-Compose:**  
```bash
docker-compose  # (type 'y' to confirm)
```  
**Install Make:** (Note: Often pre-installed)  
```bash
make
```  
**Clone Mythic Repository:**  
```bash
git clone https://github.com/its-a-feature/Mythic
```

## 4. Mythic Installation  
**Navigate to Directory:**  
```bash
cd Mythic
```  
**Run Install Script:**  
```bash
./install_docker-ubuntu.sh
```  
**Run Make:**  
- *Troubleshooting:* If Docker daemon error appears:  
  ```bash
  systemctl status docker  # Check service status
  systemctl restart docker # Restart service
  ```  
- Execute `make` from the `Mythic` directory.  
**Start Mythic:**  
```bash
./mythic-cli start
```

## 5. Firewall Hardening (Vultr)  
**Goal:** Restrict access to only analyst/target machines.  
**Create New Firewall Group:**  
1. Navigate to *Network → Firewall → Add Firewall Group*.  
2. Name group (e.g., `your-own-mythic-firewall`).  
**Configure Inbound Rules:**  
- Rule 1 (Analyst Access): Allow TCP Port Range `1-65535` from **My IP**.  
- Rule 2 (Windows Agent): Allow TCP Port Range `1-65535` from your Windows server's Public IP.  
- Rule 3 (Linux Agent): Allow TCP Port Range `1-65535` from your Linux server's Public IP.  
**Apply Firewall:**  
1. Go to Mythic server's *Settings → Firewall*.  
2. Select the new firewall group → *Update Firewall Group*.  

## 6. Accessing the Mythic Web UI  
**Access URL:**  
```
https://<SERVER_PUBLIC_IP>:7443
```  
> **Note:** You must type `https://`.  
**Credentials:**  
- Username: `mythic_admin`  
- Password retrieval:  
  ```bash
  cd Mythic
  cat .env  # Find MYTHIC_ADMIN_PASSWORD value
  ```  

## 7. Initial UI Configuration  
**Set Operation Name:**  
1. In bottom-left corner, click default operation name (e.g., "Operation Cima").  
2. Change to custom name (e.g., `your-own-operation-name`).
