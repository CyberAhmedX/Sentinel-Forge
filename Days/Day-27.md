# DAY 27: Investigate RDP Brute Force Attack  
This guide details the step-by-step methodology for investigating an RDP Brute Force alert in Kibana and determining its success and impact.  

## 1. Initial Triage & osTicket Integration  
**Find Alert**: In Kibana, navigate to `Security → Alerts`. Filter for "RDP Brute Force" alerts.  
**Configure osTicket Action**: To ensure RDP alerts create tickets (like the SSH rule):  
1. Navigate to `Security → Rules` and find the **SSH Brute Force Attempt** rule.  
2. Click `Edit rule settings → Actions tab`.  
3. Copy the entire XML Body from the webhook action.  
4. Go back to `Security → Rules` and find the **RDP Brute Force** rule.  
5. Click `Edit rule settings → Actions tab`.  
6. Select the **Webhook** action.  
7. Paste the XML body copied from the SSH rule.  
8. **Save** the rule.  

## 2. The 4-Step Investigation Methodology  
Answer these key questions for any brute-force alert:  
1. Is the IP known to perform brute-force activity?  
2. Were any other users affected by this IP?  
3. Were any login attempts successful?  
4. If successful, what activity occurred after the login?  

## 3. Case Study 1: Unsuccessful Attack (Internet Noise)  
**Alert**: RDP Brute Force from Source IP `81.163.20.41` targeting `administrator`.  

### Step 1: IP Reputation Analysis  
- **Tools**: Check IP in threat intelligence tools.  
- **AbuseIPDB**: 55 reports (48% confidence), reports for "RDP Brute Force" against `administrator`.  
- **GreyNoise**: "Unknown Intent" + "RDP Crawler" tag (indicates mass-scanning).  
- **Verdict (Q1)**: IP is known for RDP scanning.  

### Step 2: Scope (Affected Users)  
- **Kibana Query**: `source.ip : "81.163.20.41"`  
- **Result**: `user.name` shows 100% attacks against `administrator`.  
- **Verdict (Q2)**: Only one user targeted.  

### Step 3: Success (Successful Logins)  
- **Kibana Query**: `source.ip : "81.163.20.41" AND event.code : 4624`  
- **Result**: No results found.  
- **Verdict (Q3)**: Attack unsuccessful.  

### Step 4: Post-Login Activity  
- **Action**: None (attack failed).  
- **Verdict (Q4)**: N/A.  
**Conclusion**: False Positive (internet noise). Close the alert.  

## 4. Case Study 2: Successful Attack (Attacker IP)  
**Alert**: RDP Brute Force from Source IP `181.215.118.251` (Day 21 attacker IP).  

### Step 1: IP Reputation Analysis  
- **AbuseIPDB**: Reports for "Brute Force" and "Web Spam".  
- **GreyNoise**: "IP not observed" (indicates targeted attack).  
- **Verdict (Q1)**: High-priority targeted IP.  

### Step 2: Scope (Affected Users)  
- **Kibana Query**: `source.ip : "181.215.118.251"`  
- **Result**: Only `administrator` targeted (ignore host accounts ending in `$`).  
- **Verdict (Q2)**: Only one user targeted.  

### Step 3: Success (Successful Logins)  
- **Kibana Query**: `source.ip : "181.215.118.251" AND event.code : 4624`  
- **Result**: 3 events found.  
- **Verdict (Q3)**: Attack successful.  

### Step 4: Post-Login Activity (Timeline Building)  
**Actions**:  
1. **Timeline Start**: Note first successful logon timestamp (e.g., `Aug 12, 2024 at 19:56:05 UTC`).  
2. **Find Logon ID**:  
   - Expand the `4624` event.  
   - Copy `winlog.logon.id` (e.g., `0x7b...`).  
3. **Analyze Session 1**:  
   - Query: `host.name : "my-win-your-own-handle" AND user.name : "administrator" AND winlog.logon.id : "YOUR_FIRST_LOGON_ID"`  
   - **Timeline**:  
     - `Event 4624`: Successful Logon  
     - `Event 4672`: Special Privileges Assigned  
     - `Event 4634`: Account Logged Off  
   - **Analysis**: Automated tool verified credentials → logged out.  
4. **Analyze Session 2**:  
   - Query: `host.name : "my-win-your-own-handle" AND user.name : "administrator" AND winlog.logon.id : "YOUR_SECOND_LOGON_ID"`  
   - **Analysis**: Long session ending with `Event 4634` (Log Off).  
5. **Investigation Window**:  
   - **Start**: First successful login (e.g., `Aug 12, 19:56:05 UTC`)  
   - **End**: Final logoff (e.g., `Aug 13, 11:33:04 UTC`)  
**Verdict (Q4)**: Investigate process creation/persistence/C2/exfiltration in this window.  

## 5. Configuration: Set Time Zone to UTC  
**Action**:  
1. Go to `Stack Management → Advanced Settings`.  
2. Search for **Time zone**.  
3. Change from `Browser` to `UTC`.  
4. Save and reload.  
**Importance**: Ensures standardized timestamps across analysts.
