# Day 12 Ubuntu Server Installation

## 1. Vultr Ubuntu Server Deployment
• **Access Vultr:** Deploy a new server.  
• **Type/Location:** Select Cloud Compute (Shared CPU) in the standard, shared location.  
• **Image:** Select Ubuntu 24.04.  
• **Size:** Select the minimal resource plan (e.g., 1 vCPU, 1 GB RAM).  
• **Networking:** DO NOT attach the server to the Virtual Private Cloud (VPC). The goal is to expose SSH (Port 22) to the internet.  
• **Hostname (Required Naming Convention):** Set the Hostname using the defined format: `[Your_Choice_Prefix]-Linux-[Your_Handle]` (e.g., `lab-Linux-johnny`)  

## 2. Server Preparation and Access
• **Access Server:** Wait for deployment, then use SSH to connect as root using the server's Public IP Address and password.  
• **Update System:**  
  ```bash
  apt-get update
  apt-get upgrade -y
  ```
• **Log Location:** Authentication logs are stored in the `/var/log` directory. The critical file for reviewing SSH brute force attempts is: `/var/log/auth.log`.  

## 3. Real-time Brute Force Observation
• **Purpose:** By leaving the server exposed for a short time, external automated bots begin attacking the SSH port, generating logs.  
• **View Raw Logs:** Display the full content of the authentication log:  
  ```bash
  cat /var/log/auth.log
  ```
• **Filtering for Failed Attempts:** Use `grep` to filter for failed password attempts for the root user:  
  ```bash
  cat /var/log/auth.log | grep -i failed | grep -i root
  ```
• **Extracting Source IPs:** Use the `cut` command to parse the field containing the source IP address (which, in this specific log format, is field 9 when delimited by a space):  
  ```bash
  cat /var/log/auth.log | grep -i failed | grep -i root | cut -d ' ' -f 9
  ```
  *(This provides a list of IP addresses attempting brute force.)*
