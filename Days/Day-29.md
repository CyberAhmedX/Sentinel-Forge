# DAY 29: Elastic Defend Setup Tutorial  
This guide details the hands-on steps for installing and configuring Elastic Defend (Elastic's EDR solution) to protect endpoints and automatically respond to threats.  

## 1. Add Elastic Defend Integration  
- **Navigate**: In Kibana, click the hamburger menu, scroll to the bottom, and select **Integrations**.  
- **Find Integration**: Search for and select **Elastic Defend**.  
- **Add Integration**:  
  - Click **Add Elastic Defend**.  
  - **Name**: `your-own-edr-handle`  
  - **Description**: `Elastic EDR - your-own-30day-challenge`  

## 2. Configure Integration Policy  
- **Configuration Type**:  
  - **Data Collection**: Select the desired level of EDR.  
    - **Traditional Endpoints**: For desktops, laptops, and VMs.  
    - **Cloud Workloads**: For Linux servers or Kubernetes.  
- **Policy**: Select **Complete EDR**. *(Note: Requires a free trial. Includes Essential EDR plus full telemetry)*.  
- **Assign to Host**:  
  - In the **"Add this integration to"** section, select **Existing hosts**.  
  - Choose the agent policy for your Windows server (e.g., `my-window-policy`).  
- **Save and Deploy**:  
  - Click **Save and Continue**.  
  - Click **Save and deploy changes**.  

## 3. Verify Endpoint Installation  
- **Navigate**: Go to **Security → Manage → Endpoints**.  
- **Verify**: Confirm your Windows server endpoint is listed and shows **"Elastic Defend"** running.  
- **Host Isolation**:  
  - Click **Actions** on the right side of your endpoint.  
  - The **Isolate Host** option is now available *(trial/paid feature)*.  

## 4. Test EDR Prevention  
- **Trigger Malware**:  
  1. Log into the Windows server (via RDP or console).  
  2. Locate the Mythic C2 agent from the previous challenge (e.g., `mydfir-30.exe`).  
  3. **Action**: Attempt to execute the file.  
- **Observe Prevention**:  
  - The execution will be blocked.  
  - A Windows notification appears: *"Malware alert Elastic Security prevented [filename]"*.  
- **Investigate (Kibana)**:  
  - Go to the **Discover** tab.  
  - Set time to **Last 15 minutes**.  
  - **Query**: `malware`  
  - **Result**: A **"Malware prevention alert"** will appear.  
  - **Analysis**: Expand the event to see:  
    - `event.code`: `malicious_file`  
    - `file.path`: Full path to the C2 agent  
    - `file.quarantine_path`: Where the file was moved  
    - `file.hash`: Hash of the malicious file  

## 5. Configure Automated Response (Host Isolation)  
- **Find Rule**: Go to **Security → Alerts** → Open the **"Malware prevention alert"** → Click **View details**.  
- **Edit Rule**:  
  - Click the **Malware prevention alert** title.  
  - Select **Edit rule setting**.  
- **Add Action**:  
  - Go to the **Actions** tab.  
  - **Action type**: `Elastic Defend`  
  - **Response Action**: `Isolate the host`  
  - **Comment**: `Testing automated isolation`  
- **Save**: Click **Save changes**.  

## 6. Test Automated Isolation Workflow  
- **Prepare Target**:  
  1. On the Windows server, open `cmd.exe`.  
  2. Start an infinite ping: `ping 8.8.8.8 -t` (monitors connectivity).  
- **Re-Arm Trigger**:  
  1. Re-download the malware (original was quarantined).  
  2. Open **PowerShell**.  
  3. Use the `invoke-webrequest` command from Day 21 to re-download the C2 agent.  
- **Observe Workflow**:  
  1. **Prevention**: Elastic Defend detects/deletes the file upon download.  
  2. **Alert**: Triggers the **"Malware prevention alert"** rule in Kibana.  
  3. **Isolation**: The rule isolates the host.  
  4. **Result**: After minutes, `ping 8.8.8.8 -t` fails with **"General failure"** (host offline).
