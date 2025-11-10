# DAY 24: osTicket Setup Tutorial  
*This guide details the hands-on steps for deploying a Windows server, installing a XAMPP stack (Apache, MySQL, PHP), and configuring the osTicket ticketing system.*

## 1. Vultr VM Deployment (osTicket Server)  
**Deploy New Server:**  
- **Server Type:** Cloud Compute, Shared CPU.  
- **Operating System:** Windows Server 2022.  
- **Server Size:** Basic plan with 2GB RAM.  
- **Additional Options:** Disable Auto Backups and IPv6.  
- **Server Hostname:** Set a new hostname (e.g., `your-own-osticket-handle`).  
- **Firewall:** Attach your previously created firewall group.  
- **Action:** Deploy the server and connect via RDP.  

## 2. XAMPP Stack Installation  
**Download:**  
- Search for "XAMPP" and download installer from [apachefriends.org](https://apachefriends.org).  

**Install:**  
1. Run installer, accepting defaults.  
   - *Default installation path:* `C:\xamp`  
2. **Configure `properties.ini`:**  
   - Navigate to `C:\xamp`  
   - Open `properties.ini` with a text editor.  
   - Change `apache_domain=localhost` → `apache_domain=`  
   - Save and close.  
3. **Configure phpMyAdmin:**  
   - Navigate to `C:\xamp\phpMyAdmin`  
   - Create backup of `config.inc.php`.  
   - **Initial Change:** Open `config.inc.php` and change:  
     `$cfg['Servers'][$i]['host'] = '127.0.0.1'` → `$cfg['Servers'][$i]['host'] = ''`  
   - **Troubleshooting:** This will initially fail. Revert to `127.0.0.1`, save, and access phpMyAdmin to grant remote privileges.  

## 3. Windows Firewall Configuration  
**Goal:** Allow inbound web traffic.  
**Action:**  
1. Open *Windows Defender Firewall with Advanced Security*.  
2. Select **Inbound Rules** → **New Rule...**  
   - **Rule Type:** Port  
   - **Protocol and Ports:** TCP, Specific ports: `80, 443`  
   - **Action:** Allow the connection  
   - **Name:** `Inbound 80, 443`  

## 4. Secure MySQL & Grant Remote Access  
**Start Services:**  
- Open XAMPP Control Panel → Start **Apache** and **MySQL**.  

**Access phpMyAdmin:**  
- In XAMPP panel, click *Admin* next to Apache → Select "phpMyAdmin" from the webpage.  

**Grant Privileges:**  
1. Click **User accounts** tab.  
2. Edit **root** account (for localhost):  
   - Under *Login Information*, change **Host name**:  
     `localhost` → *Use text field:* `[Your Server's Public IP Address]`  
   - Set a new password (e.g., `your-secure-password`) → Click **Go**.  
3. Repeat for **pma** user account (same password).  

**Update Config File (Final):**  
- Open `C:\xamp\phpMyAdmin\config.inc.php`:  
  ```php
  $cfg['Servers'][$i]['host'] = '';
  $cfg['Servers'][$i]['password'] = 'your-secure-password'; // root user
  $cfg['Servers'][$i]['controlpass'] = 'your-secure-password'; // pma user
  ```  
- Restart Apache and MySQL services in XAMPP.  

## 5. Install osTicket  
**Download osTicket:**  
- Visit [osticket.com](https://osticket.com) → Download self-hosted version (e.g., v1.18.1).  

**Stage Files:**  
1. Extract downloaded `.zip` file.  
2. Navigate to `C:\xamp\htdocs`.  
3. Create folder: `osTicket`.  
4. Copy `scripts` and `upload` folders from extracted ZIP → `C:\xamp\htdocs\osTicket`.  

**Prepare Config File:**  
- Go to `C:\xamp\htdocs\osTicket\upload\include`  
- Rename `ost-sampleconfig.php` → `ost-config.php`.  

## 6. osTicket Web Installer  
**Create Database:**  
1. Access phpMyAdmin → Click **New**.  
2. Database name: `your-own-db-name` (e.g., `mydfir-30-day-db`) → Click **Create**.  

**Run Installer:**  
- Open browser → Navigate to `http://[Your-Server-IP]/osTicket/upload`  

**Helpdesk Settings:**  
- Helpdesk Name: `your-own-handle-Support`  
- Default Email: e.g., `test@test.com`  

**Admin User:**  
- Create admin username/password.  

**Database Settings:**  
- MySQL Hostname: `[Your-Server-IP]`  
- MySQL Database: `your-own-db-name`  
- MySQL Username: `root`  
- MySQL Password: `your-secure-password`  
- **Action:** Click **Install Now**.  

## 7. Post-Installation Steps  
**Secure Config File:**  
1. Open PowerShell **as Administrator**.  
2. Navigate: `cd C:\xamp\htdocs\osTicket\upload\include`  
3. Run: `icacls ost-config.php /reset`  

**Delete Setup Directory:**  
1. Go to `C:\xamp\htdocs\osTicket\upload`  
2. Delete the `setup` folder.  

**Access Panels:**  
- **Staff Control Panel (Admin):** `http://[Your-Server-IP]/osTicket/upload/scp`  
- **Client Support Portal:** `http://[Your-Server-IP]/osTicket/upload`  
- **Action:** Log into `/scp` with admin credentials to verify setup.
