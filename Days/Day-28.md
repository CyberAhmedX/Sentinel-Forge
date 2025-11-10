# DAY 28: Investigate Mythic Agent  
*This guide details the methodology for investigating C2 (Command and Control) activity using Kibana, focusing on how to trace the activity from an unknown starting point.*  

## 1. C2 Detection Strategies  
**Network Telemetry:**  
- Top Talkers: C2 sessions often involve significant back-and-forth traffic, placing them in the "top 10 talkers" (most bytes transferred).  
- Heartbeat: C2 agents send regular, automated "heartbeat" check-ins. Tools like Rita (Real Intelligence Threat Analytics) can detect this, though it is not used in this challenge.  

**Endpoint Telemetry (Sysmon):**  
- The most effective method is correlating Process Creations (Sysmon Event ID 1) with Network Connections (Sysmon Event ID 3).  
- **This is the strategy that will be used for this investigation.**  

## 2. Investigation: Start from the Dashboard  
**Navigate:**  
Go to the Suspicious Activity dashboard created on Day 22.  

**Analyze Panels:**  
- *Process Initiated Network Connections*: This panel is the best starting point.  
- Look for unusual executables initiating outbound connections.  
- **Key Finding:** An executable running from `C:\Users\Public\Downloads` is seen making an outbound connection to an external IP on port 9999.  
- **Analyst Note:** An executable running from a public download directory is highly suspicious, regardless of its name. This becomes our pivot point.  

**Pivot to Discover:**  
- Copy the Destination IP (e.g., `155.138.158.156`) from the suspicious connection.  

## 3. Investigation: Build Timeline with Process GUID  
**Initial Query (Discover):**  
Search for the destination IP and event code 3 (Network Connection):  
```  
event.code : 3 AND windlog.eventdata.DestinationIp : "155.138.158.156"  
```  

**Find the "Parent" Process:**  
1. Expand the event details.  
2. **Crucial Pivot:** Copy the `windlog.eventdata.ProcessGuid`. This unique ID correlates all events spawned by this single process (in this case, the `powershell.exe` process).  

**Build Timeline (Query 2):**  
1. Remove the old query and search for the ProcessGuid (remember to use quotes).  
```  
"PROCESS_GUID_HERE"  
```  
2. Sort by timestamp, "Old to New."  

**Analyze the Timeline (PowerShell Session):**  
1. **Start:** The timeline begins with the user (administrator) opening `powershell.exe`.  
2. **File Create (Event 11):** Scrolling down, a "File Created" event is seen. The `powershell.exe` process created a file named `servicehost-your-own-handle.exe` in the `C:\Users\Public\Downloads` directory.  
3. **Network Connection (Event 3):** Immediately after, the network connection events appear as PowerShell downloads the file.  
4. **File Executable Detected (Event 29):** Sysmon detects the creation of the new executable file and logs its hash (SHA1).  
5. **Process Create (Event 1):** The `powershell.exe` process executes the newly downloaded `servicehost-your-own-handle.exe`.  

## 4. Investigation: Pivot to the C2 Agent Process  
**Find C2 Agent GUID:**  
- In the "Process Create" event (Step 5 above), find the `ProcessGuid` for the newly created process (`servicehost-your-own-handle.exe`). This is the GUID for the C2 agent itself.  

**Build Timeline (Query 3):**  
1. Remove the old query and search for the C2 agent's ProcessGuid.  
```  
"C2_AGENT_GUID_HERE"  
```  
2. Sort by timestamp, "Old to New."  

**Analyze the C2 Agent Timeline:**  
- The timeline begins with the Process Create event.  
- **Network Connection (Event 3):** You will see the C2 agent making its own network connection to the C2 server (e.g., on port 80). This is the C2 "heartbeat" or callback.  
- **Process Create (Event 1):** You can see if the C2 agent spawns any new processes, such as `cmd.exe` or `ipconfig.exe`.  

**Note on C2 Commands:**  
- **Internal Commands:** Commands that run within the agent (like `netstat`) may not generate new endpoint logs. They are only visible in network traffic (which is often encrypted).  
- **Spawned Commands:** Commands that spawn a new process (like `shell ipconfig`) will create a `cmd.exe` process with the C2 agent as the Parent Process. This is a critical detection.  

## 5. Finalize Rule and Test  
**Update Rule Schedule:**  
1. Go to `Security → Rules → your-own-handle-Mythic-C2-Apollo-Agent-Detected`.  
2. Change the Schedule to run every 1 minute.  

**Add osTicket Action:**  
1. Copy the XML body from the SSH Brute Force rule.  
2. Edit the Mythic C2 rule, go to the Actions tab, add the Webhook connector, and paste in the XML body.  
3. Save the rule.  

**Trigger and Verify:**  
1. On the Windows server, disable Windows Defender real-time protection or add an exclusion for the `C:\Users\Public\Downloads` folder.  
2. Re-run the PowerShell download command.  
3. Execute the new C2 agent.  
4. A new alert will appear in Kibana.  
5. A new ticket will be created in osTicket.  

**Verification (Post-Attack):**  
1. Interact with the C2 agent (e.g., run `shell ipconfig`).  
2. Refresh Kibana Discover. You will see a new Process Create event for `cmd.exe`, with the C2 agent (e.g., `mydfir-30.exe`) listed as the `ParentImage`. This confirms the C2 activity.
