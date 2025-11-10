# Day 3: Elasticsearch Setup Guide

## Vultr VPC and Server Deployment

### VPC Creation
- Navigate to **Network → VPC 2.0**
- Select a consistent location (must match VM location)
- Set IPv4 Range to: `172.31.0.0/24`

### VM Deployment
1. Deploy new server (VM)  
2. Select OS: **Ubuntu 22.04 LTS**  
3. Configure:  
   - Set server size  
   - Disable Auto Backups  
   - Attach server to the VPC 2.0 Network  
4. Note critical details:  
   - Private IP (e.g., `172.31.0.3`)  
   - Public IP  
   - Set hostname  

---

## Server Access and Installation

### Access & Update
```bash
ssh root@<PUBLIC_IP>
apt-get update && apt-get upgrade -y
```

### Install Elasticsearch
1. Download Deb package:  
   ```bash
   wget <Elasticsearch_Deb_Package_URL>
   ```
2. Install package:  
   ```bash
   dpkg -i elasticsearch-x.x.x-amd64.deb
   ```

### Security Action (Crucial)
- **Immediately copy and save**:  
  - Elastic superuser password  
  - Entire Security Auto-configuration section  

---

## Configuration and Service Control

### Configure Elasticsearch
1. Edit config file:  
   ```bash
   nano /etc/elasticsearch/elasticsearch.yml
   ```
2. Modify network settings:  
   ```yml
   network.host: <SERVER_PUBLIC_IP>  # Uncomment and set
   ```

### Firewall Hardening (Vultr UI)
1. Create new **Firewall Group**  
2. Edit SSH rule:  
   - Restrict access to **"My IP"** only  
3. Apply Firewall Group to the VM  

### Service Management
```bash
systemctl daemon-reload
systemctl enable elasticsearch.service
systemctl start elasticsearch.service
```

### Verification
```bash
systemctl status elasticsearch.service
```
✅ Confirm status: **"active (running)"**
```
