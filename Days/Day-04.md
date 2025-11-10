# Day 4: Kibana Setup Guide

## 1. Kibana Installation
```bash
# Download .deb package (replace URL with current version)
wget https://artifacts.elastic.co/downloads/kibana/kibana-x.x.x-amd64.deb

# Install package (use Tab for filename completion)
dpkg -i kibana-*-amd64.deb
```

## 2. Configuration
```bash
nano /etc/kibana/kibana.yml
```
**Required Edits:**
```yaml
server.host: "your-server-public-ip"  # Replace with actual IP
server.port: 5601                    # Ensure uncommented
```
*(Save with Ctrl+X → Y → Enter)*

## 3. Service Management & Firewall
```bash
systemctl daemon-reload
systemctl enable --now kibana.service
systemctl status kibana.service
```

### Firewall Configuration:
**Vultr Web UI:**
- Allow TCP ports 1-65535 from your IP  
**Server Terminal:**
```bash
ufw allow 5601
```

## 4. Connect to Elasticsearch
### Generate Enrollment Token:
```bash
cd /usr/share/elasticsearch/bin
./elasticsearch-create-enrollment-token --scope kibana  # COPY token
```

### Browser Setup:
1. Access `http://<your-server-ip>:5601`
2. Paste enrollment token → "Configure Elastic"

### Verification Code:
```bash
cd /usr/share/kibana/bin
./kibana-verification-code  # COPY 6-digit code
```
3. Paste code in browser → Log in as `elastic` with Day 3 password

## 5. Encryption Keys Setup
```bash
cd /usr/share/kibana/bin
./kibana-encryption-keys generate  # SAVE all 3 keys
```

### Add Keys to Keystore:
```bash
./kibana-keystore add xpack.encrypted_saved_objects.encryptionKey
# When prompted, paste first key value

# Repeat for other keys:
./kibana-keystore add xpack.reporting.encryptionKey
./kibana-keystore add xpack.security.encryptionKey
```

### Finalize:
```bash
systemctl restart kibana.service
```
**Verification:**  
Refresh Kibana → Confirm "API integration key required" warning disappears
