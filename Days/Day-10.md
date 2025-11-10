## Day 10 Elasticsearch Ingest Data

### 1. Adding Sysmon Integration
1. **Access Kibana**: Navigate to *Add Integrations* on the homepage or via the left-hand menu.
2. **Search**: Search for and select the `Custom Windows Event Log` package.
3. **Configure Integration**: Click *Add custom windows event logs*.
   - **Integration Name**: `my-dfir-win-Sysmon`
   - **Channel Name (Crucial)**: Get the exact channel name from the Windows Server's Event Viewer.
     - Open Event Viewer on the Windows Server.
     - Navigate to `Applications and Services Logs` → `Microsoft` → `Windows` → `Sysmon` → `Operational`.
     - Right-click `Operational` → `Properties` → Copy the **Full Name**.
     - Paste the copied channel name into the Kibana field (e.g., `Microsoft-Windows-Sysmon/Operational`).
   - **Assign Policy**: Select the existing host policy (`my-dfir-Windows-policy`).
4. **Action**: Click *Save and deploy changes*.

### 2. Adding Windows Defender Integration (Filtered)
1. **Action**: Return to the Integrations page and add another instance of the `Custom Windows Event Log` package.
2. **Integration Name**: `my-dfir-win-Defender`
3. **Channel Name (Crucial)**: Get the channel name for Windows Defender events:
   - Navigate to `Applications and Services Logs` → `Microsoft` → `Windows` → `Windows Defender` → `Operational`.
   - Copy the **Full Name** (e.g., `Microsoft-Windows-Windows Defender/Operational`).
4. **Filter Event IDs**: Scroll down to *Advanced options* and enter a comma-separated list of event IDs to include:
   - **Event IDs**: `1116, 1117, 5001`
     - *Note*: These IDs cover malware detection, actions taken, and real-time protection being disabled.
5. **Assign Policy**: Select the existing host policy (`my-dfir-Windows-policy`).
6. **Action**: Click *Save and deploy changes*.

### 3. Critical Troubleshooting (Log Ingestion Failure)
Logs often fail to ingest initially because the firewall blocks communication from the Windows Agent to Elasticsearch.
- **Symptom**: In `Fleet` → `Agents`, the Windows Agent's **CPU and Memory metrics show `N/A`**. This means the Agent cannot communicate with the Elasticsearch instance on port `9200`.
- **Solution (Vultr Firewall)**:
  - Log in to the Vultr web UI and go to your **Firewall Group**.
  - Add a new **Inbound Rule** to allow the Windows Agent to send data to the ELK Server:
    - **Protocol**: `TCP`
    - **Port**: `9200`
    - **Source**: For quick setup, set the source to `Anywhere (0.0.0.0/0)`. *(In a production environment, this should be restricted to the Windows Server's IP address)*.
- **Restart Service**: After applying the Vultr firewall rule, restart the Elastic Agent service on the Windows Server:
  - Open `Services` → Right-click `Elastic Agent` → `Restart`.

### 4. Final Verification
1. **Check Metrics**: Return to `Fleet` → `Agents` and confirm the **CPU and Memory fields** are now showing live metrics instead of `N/A`.
2. **Verify Logs (Kibana Discover)**: Navigate to `Discover`.
   - **Sysmon**: Search for `winlog.event_id: 1` and verify the `event.provider` is `Microsoft-Windows-Sysmon`.
   - **Defender**: Search for `winlog.event_id: 5001` and verify the `event.provider` is `Microsoft-Windows-Windows Defender`.
