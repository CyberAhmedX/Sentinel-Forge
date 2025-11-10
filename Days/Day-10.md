# Day 10 Elasticsearch Ingest Data

## 1. Adding Sysmon Integration
1. **Access Kibana**: Navigate to Add Integrations on the homepage or via the left-hand menu.
2. **Search**: Search for and select the Custom Windows Event Log package.
3. **Configure Integration**: Click *Add custom windows event logs*.
   - `Integration Name`: my-dfir-win-Sysmon
   - `Channel Name (Crucial)`: Get the exact channel name from the Windows Server's Event Viewer:
     - Open Event Viewer on the Windows Server.
     - Navigate to `Applications and Services Logs $\\rightarrow$ Microsoft $\\rightarrow$ Windows $\\rightarrow$ Sysmon $\\rightarrow$ Operational`.
     - Right-click *Operational* $\\rightarrow$ *Properties* $\\rightarrow$ Copy the **Full Name**.
     - Paste the copied channel name into Kibana (e.g., `Microsoft-Windows-Sysmon/Operational`).
   - `Assign Policy`: Select the existing host policy (`my-dfir-Windows-policy`).
4. **Action**: Click *Save and deploy changes*.

## 2. Adding Windows Defender Integration (Filtered)
1. **Action**: Return to the Integrations page and add another instance of the Custom Windows Event Log package.
2. `Integration Name`: my-dfir-win-Defender
3. `Channel Name (Crucial)`: Get the channel name for Windows Defender events:
   - Navigate to `Applications and Services Logs $\\rightarrow$ Microsoft $\\rightarrow$ Windows $\\rightarrow$ Windows Defender $\\rightarrow$ Operational`.
   - Copy the **Full Name** (e.g., `Microsoft-Windows-Windows Defender/Operational`).
4. **Filter Event IDs**: Scroll down to *Advanced options* and enter a comma-separated list of event IDs to include:
   - `Event IDs`: `1116, 1117, 5001`  
     *(Note: These cover malware detection, actions taken, and real-time protection being disabled)*.
5. `Assign Policy`: Select the existing host policy (`my-dfir-Windows-policy`).
6. **Action**: Click *Save and deploy changes*.

## 3. Critical Troubleshooting (Log Ingestion Failure)
Logs often fail to ingest because the firewall blocks communication from the Windows Agent to Elasticsearch.
- **Symptom**: In `Fleet $\\rightarrow$ Agents`, the Windows Agent's CPU and Memory metrics show `N/A` (indicates Agent can't communicate with Elasticsearch on port `9200`).
- **Solution (Vultr Firewall)**:
  1. Log in to the Vultr web UI and go to your Firewall Group.
  2. Add a new **Inbound Rule**:
     - `Protocol`: TCP
     - `Port`: 9200
     - `Source`: For quick setup, use `Anywhere (0.0.0.0/0)` *(Production: Restrict to the Windows Server's IP)*.
- **Restart Service**: After applying the firewall rule:
  - On Windows Server: Open `Services` $\\rightarrow$ Right-click `Elastic Agent` $\\rightarrow$ `Restart`.

## 4. Final Verification
1. **Check Metrics**: In `Fleet $\\rightarrow$ Agents`, confirm CPU/Memory fields show live metrics (not `N/A`).
2. **Verify Logs (Kibana Discover)**:
   - **Sysmon**: Search `winlog.event_id: 1` and verify `event.provider` is `Microsoft-Windows-Sysmon`.
   - **Defender**: Search `winlog.event_id: 5001` and verify `event.provider` is `Microsoft-Windows-Windows Defender`.
```
