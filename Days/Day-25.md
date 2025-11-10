### DAY 25: osTicket + ELK Integration  
This guide details the hands-on steps for integrating the Elastic (ELK) stack with osTicket to automatically generate tickets from security alerts.

---

#### 1. osTicket: Generate API Key  
**Navigate**: Log into the osTicket Staff Control Panel (`/scp`), go to `Admin Panel → Manage → API`.  

**Add New API Key**:  
- **IP Address**: Enter the Private IP of your Elastic (ELK) server (e.g., `172.31.0.3`). This is required to authorize requests from Elastic.  
- **Services**: Check the box for **Can Create Tickets**.  
- **Internal Notes**: Add a descriptive note (e.g., `your-own-elastic-connector`).  
- **Action**: Click **Add Key**.  

⚠️ **Critical**: Copy the generated API Key immediately and save it in a secure location. It will not be shown again.  

---

#### 2. Kibana: Enable Trial & Create Connector  
**Enable Trial**:  
- The API connector feature requires a trial license.  
- In Kibana, go to `Stack Management → Manage License`.  
- Click **Start a 30-day trial**.  

**Navigate to Connectors**:  
- Go back to `Stack Management → Alerts and Insights → Connectors`.  

**Create Connector**:  
- Click **Create connector**.  
- Select the **Webhook** connector type.  

**Configure Connector**:  
- **Name**: `your-own-handle-osTicket-Connector`  
- **Method**: `POST`  
- **URL**: Enter the osTicket API endpoint.  
  - **Note**: The URL must use the server's Private IP and the correct API path:  
    `http://<osTicket_Private_IP>/osTicket/upload/api/tickets.xml`  
- **Headers**: Add a custom header for authentication:  
  - **Key**: `X-API-Key`  
  - **Value**: `PASTE_THE_API_KEY_FROM_STEP_1`  
- **Action**: Click **Save**.  

---

#### 3. Kibana: Configure Connector JSON Body  
**Define Payload**:  
- After saving, you are prompted to test the connector.  
- In the **Body** text box, provide the XML payload that osTicket expects.  
  - **Note**: This is an XML payload, **not JSON**.  

**Payload Template**:  
Paste the following XML structure into the **Body**:  

```xml
<ticket>
  <name>Elastic SIEM</name>
  <email>alerts@your-domain.com</email>
  <subject>my-dfir-30day-challenge-your-own-handle</subject>
  <message>This is a test alert from Elastic.</message>
  <ip>127.0.0.1</ip>
  <topicId>1</topicId>
  <autorespond>true</autorespond>
</ticket>
```

⚠️ **Important**: The subject line must be customized per the challenge instructions.  

---

#### 4. Network Troubleshooting (VPC)  
**Issue**: A test run may fail due to a "request failed time out" error. This indicates a network connection issue between the Elastic and osTicket servers.  

**Diagnosis**:  
1. SSH into the Elastic (ELK) server.  
2. Ping the osTicket server’s private IP (e.g., `ping 172.31.0.5`).  
3. If it fails, the osTicket server’s network adapter is not configured for the private VPC.  

**Resolution (on osTicket Windows Server)**:  
1. Go to `Control Panel → Network and Internet → Change adapter settings`.  
2. Identify the adapter with an incorrect IP (e.g., a `169.x.x.x` address).  
3. Right-click → **Properties → Internet Protocol Version 4 (TCP/IPv4)**.  
4. Select **Use the following IP address**:  
   - **IP address**: Enter the correct static Private IP (e.g., `172.31.0.5`).  
   - **Subnet mask**: `255.255.255.0`  
5. Click **OK** and close.  

---

#### 5. Test and Verify Integration  
**Rerun Test**:  
- Go back to the Kibana connector test page and click **Run test**. The test should now show as successful.  

**Verify Ticket**:  
1. Go to the osTicket Agent Panel (`/scp`).  
2. A new ticket will be present in the **Open** queue with the subject line you defined (e.g., `my-dfir-30day-challenge-your-own-handle`), confirming the integration is working.
