## Day 13 How To Install Elastic Agent on Ubuntu

### 1. Agent Policy Creation (Kibana UI)
1. **Access Fleet**: In Kibana, navigate to Management → Fleet.
2. **Create Policy**: Go to Agent Policies and click *Create agent policy*.
   - **Policy Name**: Enter your own choice name (e.g., `ssh-server-linux-policy`).
   - **Action**: Click *Create agent policy*.
3. **Verify Data Stream**: Review the new policy to confirm that the default System integration covers the Ubuntu authentication log file (`/var/log/auth.log`).

### 2. Linux Agent Enrollment
1. **Add Agent**: In Kibana, go to the Agents tab and click *Add agent*.
2. **Select Policy**: Choose the Linux policy created above.
3. **Copy Command**: Select the Linux tar deployment option and copy the provided command block.
4. **Execute Command (SSH)**: Log into your Ubuntu server via SSH and paste the command.
   - **Troubleshooting (Crucial)**: Enrollment will fail initially due to the self-signed certificate. You must re-run the command, adding the flag:  
     `--insecure`

### 3. Verification and Log Analysis
1. **Confirm Status (Kibana UI)**: Return to the Fleet interface and confirm the new Linux Agent is listed and the status shows *Connected*.
2. **Access Logs (Kibana Discover)**: Navigate to *Discover*.
   - **Filter**: Filter the results by the agent name of your Linux server (e.g., `agent.name: lab-Linux-johnny`).
3. **Analyze Brute Force Logs**:
   - **Search**: Search using keywords related to failed logins (e.g., `authentication failure`).
   - **Field Extraction**: Add the `message` field to the main data table view to easily read the full log messages from the Linux server (e.g., `Pam Unix SSH auth failure`).
   - **Extraction Example**: You can now query for a specific attacking IP address to see all failed login attempts associated with it.
```
