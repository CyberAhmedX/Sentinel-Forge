# Day 7 Elastic Agent and Fleet Server Setup

## 1. Fleet Server Deployment (Ubuntu VM)
- **Action**: Deploy a new Ubuntu server (e.g., 22.04 LTS).
- **Location/Size**: Select the same Location as the ELK server. Choose the 1 vCPU, 4 GB RAM option.
- **Networking (Crucial)**: Attach the new server to the VPC 2.0 Network.
- **Naming**: Set the Hostname (e.g., `my-dfir-fleet-server`).
- **Preparation**: SSH into the new server and update repositories:
  ```bash
  apt-get update
  apt-get upgrade -y
  ```

## 2. Fleet Policy Creation (Kibana UI)
- **Access Fleet**: In the Kibana web interface, navigate to Management $\\rightarrow$ Fleet.
- **Add Fleet Server**: Select `Add Fleet server` $\\rightarrow$ `Quick start`.
- **Name & URL**:
  - Name: Set the policy name (e.g., `my-dfir-fleet-server`).
  - URL: Enter the Fleet Server VM's Public IP Address and the default port, ensuring you use HTTPS: `https://<PUBLIC_IP>:8220`.
- **Action**: Click `Generate Fleet server policy`.

## 3. Fleet Server Installation (Ubuntu Server)
- **Command**: Copy the `sudo elastic-agent install` command provided in the Kibana UI.
- **Execution**: Paste the command into the SSH session of the Fleet Server VM and confirm to continue.

## 4. Critical Connectivity Troubleshooting (Firewalls)
Installation will initially fail due to firewall restrictions. These steps are necessary for the servers to communicate:
- **ELK Server UFW**: Allow the Fleet Server (and later Kibana) to communicate with Elasticsearch on port 9200:
  ```bash
  sudo ufw allow 9200
  ```
- **Fleet Server UFW**: Allow agents (like the Windows Server) to connect to the Fleet Server's default port:
  ```bash
  sudo ufw allow 8220
  ```

## 5. Fleet Server Policy Correction (Kibana UI)
The initial policy defaults to using Port 443, causing issues. This must be corrected:
1. **Access Settings**: In Kibana, go to Fleet $\\rightarrow$ Settings.
2. **Edit Policy**: Click the pencil icon next to the Fleet server URL.
3. **Update Port**: Change the port in the URL from `443` to the default `8220`.
4. **Save**: Click `Save and Apply Settings` $\\rightarrow$ `Save and Deploy`.

## 6. Windows Agent Enrollment
- **Create Agent Policy**: In Kibana, click `Add agent`, name the policy (e.g., `my-dfir-Windows-policy`), and select `Windows` as the host type.
- **Copy Command**: Copy the Powershell installation command provided by Kibana.
- **Execution (Windows Server)**:
  - Access the Windows Server console (Day 5 VM).
  - Open PowerShell as Administrator.
  - **Modify Command (Crucial)**: Before running the command, you must add the `--insecure` flag and manually correct the port to `8220` to bypass certificate errors and port conflicts:
    ```powershell
    ... --insecure -f https://<FLEET_IP>:8220
    ```
  - Confirm to continue the installation.

## 7. Final Verification
- **Agent Status**: In Kibana $\\rightarrow$ Fleet $\\rightarrow$ Agents, confirm the Windows Agent is `Connected`.
- **Log Data**: Navigate to `Discover` and search for logs (e.g., `event.code: 4625`) to confirm failed logon events are being collected from the Windows Server.
