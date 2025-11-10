## DAY 22: Alerts and Dashboards in Kibana (Mythic C2 activity)
This guide details the hands-on steps for creating a detection rule and a new dashboard in Kibana to identify the Mythic C2 activity generated on Day 21.

### 1. Investigation in Kibana (Discover)
**Set Time Range**: Open the "Discover" tab and set the time range to the last 30 days to ensure all events are captured.  

**Initial Query**: Search for the known payload name (e.g., `servicehost-your-own-handle.exe`).  

**Identify Key Fields**:  
→ Expand the events and identify unique indicators.  
→ **Original File Name**: The `windlog.eventdata.OriginalFileName` field shows `apollo.exe` even though the running process name is different.  
→ **Process Create**: The `event.code` for process creation is `1`.  
→ **Hashes**: The event contains hashes (`SHA1`, `MD5`, `SHA256`) for the executable.  

**Build Detection Query**:  
→ A rule can be built by combining these unique fields.  
→ The query will focus on `event.code: 1` (Process Create) combined with specific indicators from the Mythic agent.  
→ **Final Query**:  
```sql
event.code : 1 AND (windlog.eventdata.OriginalFileName : "apollo.exe" OR windlog.eventdata.Hashes : "*PASTE_SHA256_HASH_HERE*")
```

**Save Search**: Save this query as a new search (e.g., `Mythic Apollo Process Create`).  

---

### 2. Create New Detection Rule
**Navigate to Rules**: Go to `Security` → `Rules` → `Create new rule`.  

**Rule Type**: Select `Custom query`.  

**Query**: Paste the detection query created in the previous step.  

**Define Rule Details**:  
→ **Name**: `your-own-handle-Mythic-C2-Apollo-Agent-Detected`  
→ **Description**: `"This rule detects a potential Mythic C2 Apollo agent."`  
→ **Severity**: `Critical`  
→ **Schedule**:  
  → Run every: `5 minutes`  
  → Look back time: `5 minutes`  

**Action**: Activate and save the rule by clicking `Create and enable rule`.  

---

### 3. Create a New "Suspicious Activity" Dashboard
#### Panel 1: Suspicious Process Creates
**Goal**: Create a table showing process creates for `powershell`, `cmd`, or `rundll32`.  

**Query**:  
```sql
event.code : 1 AND event.provider : "Microsoft-Windows-Sysmon" AND (powershell.exe OR cmd.exe OR rundll32.exe)
```

**Visualization**:  
→ Create a new visualization using the query above.  
→ Change the visualization type to `Table`.  
→ **Add Columns (Metrics)**:  
  - `host.name`  
  - `windlog.eventdata.User`  
  - `windlog.eventdata.ParentImage`  
  - `windlog.eventdata.ParentCommandLine`  
  - `windlog.eventdata.Image`  
  - `windlog.eventdata.CommandLine`  
  - `windlog.eventdata.CurrentDirectory`  
→ **Title**: `Process Created (PowerShell, CMD, RunDLL32)`  
→ Save and add to the new dashboard.  

---

#### Panel 2: Process Initiated Network Connections
**Goal**: Create a table showing processes initiating outbound network connections, excluding known good processes (like Defender).  

**Query**:  
```sql
event.code : 3 AND event.provider : "Microsoft-Windows-Sysmon" AND windlog.eventdata.Initiated : "true" AND NOT windlog.eventdata.Image : "*msmpeng.exe*"
```

**Visualization**:  
→ Create a new visualization using the query above.  
→ Change the visualization type to `Table`.  
→ **Add Columns (Metrics)**:  
  - `windlog.eventdata.Image`  
  - `source.ip`  
  - `destination.ip`  
  - `destination.port`  
→ **Title**: `Process Initiated Network Connections`  
→ Save and add to the dashboard.  

---

#### Panel 3: Microsoft Defender Disabled
**Goal**: Create a table to show when Windows Defender is disabled (Event ID 5001).  

**Query**:  
```sql
event.code : 5001 AND event.provider : "Microsoft-Windows-Defender"
```

**Visualization**:  
→ Create a new visualization using the query above.  
→ Change the visualization type to `Table`.  
→ **Add Columns (Metrics)**:  
  - `host.name`  
  - `event.provider`  
  - `event.code`  
→ **Title**: `Microsoft Defender Disabled`  
→ Save and add to the dashboard.  

---

### 4. Finalize Dashboard
**Arrange Panels**: Organize the three new panels on the dashboard.  

**Save Dashboard**:  
→ Click `Save` in the top-right corner.  
→ **Title**: `your-own-handle-Suspicious-Activity`  
→ **Action**: Save the dashboard.
