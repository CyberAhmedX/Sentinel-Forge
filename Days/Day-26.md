# DAY 26: Investigate SSH Brute Force Attack
This guide details the methodology for investigating an SSH brute-force alert in Kibana and integrating the alert with osTicket.

## 1. Investigation Methodology (Kibana)
**Find Alert:**  
Navigate to Security → Alerts. Select a triggered SSH brute-force alert to investigate.

### Key Questions:
- **IP Reputation:** Is the source IP known to perform brute-force activities?
- **Scope:** Were any other user accounts affected by this IP?
- **Success:** Were any of the login attempts successful?
- **Post-Activity:** If successful, what activity occurred after the login?

### Tools (IP Reputation):
- Use threat intelligence platforms to check the source IP (e.g., `194.50.201.5`).  
- **AbuseIPDB:** Check for report confidence and history.  
- **GreyNoise:** Check for malicious tags (e.g., `"SSH Brute-Forcer"`).

### Investigate (Discover):
1. Go to the **Discover** tab and set the time range (e.g., *Last 30 days*).  
2. **Query 1 (Scope):**  
   Search for the source IP address. Add the `user.name` field as a column to see all users that were targeted.  
3. **Query 2 (Success):**  
   Search for the IP address and the keyword `"Accepted"` (case-sensitive) to check for successful logins.  

### Analysis:
- In this scenario, the IP was known-malicious, targeted four users (`root`, `oracle`, `guest`, `test`), and had zero successful logins.  
- The investigation can be closed as no further activity occurred.  

---

## 2. Configure osTicket Integration
**Goal:** Modify the SSH brute-force rule to automatically create a ticket in osTicket.

### Edit Rule:
1. Navigate to **Security → Rules**.  
2. Edit the **SSH Brute Force Attempt** rule.  
3. Go to the **Actions** tab and select your Webhook connector.  

### Configure Kibana `kibana.yml`:
1. On the ELK server: `nano /etc/kibana/kibana.yml`.  
2. Uncomment and set `server.publicBaseUrl`:  
   ```yaml
   server.publicBaseUrl: "http://<ELK_IP>:5601"
   ```  
3. Save the file and restart Kibana: `systemctl restart kibana.service`.  

### Configure Action Body (XML):
Use dynamic variables to populate the ticket.  

**Body Template:**
```xml
<ticket>
  <name>Elastic SIEM</name>
  <email>api@osticket.com</email>
  <subject>{{context.rule.name}}</subject>
  <message>
    Please investigate the rule: {{context.rule.name}}
    Link: {{context.rule.url}}
  </message>
  <priority>1</priority>
  <autorespond>true</autorespond>
</ticket>
```

**Action:** Save the rule.  

---

## 3. Ticket Management (osTicket)
**Review:** New tickets will now appear in the osTicket dashboard.  

**Assign:**  
- Assign the ticket to yourself (e.g., **Assign → your-own-handle**).  
- Add an internal note (e.g., `"Working on ticket"`).  

**Investigate:**  
Click the link in the ticket to go directly to the Kibana rule.  

**Close:**  
1. After investigation, resolve the ticket.  
2. **Status:** Select **Resolved**.  
3. **Response:** Add closing notes (e.g., `"Closing as test"`).  
4. Click **Post Reply**.

