# Day 15 Remote Desktop Protocol Introduction

## 1. RDP Fundamentals
- **Definition**: RDP is a proprietary Microsoft protocol used for communication between a terminal server and a client, allowing authorized users to connect remotely to another machine.
- **Protocol Details**: Works over TCP and uses the default port 3389.
- **User Benefit**: Provides remote access for troubleshooting or working remotely, offering convenience and saving commute time.
- **Security Context**: RDP abuse was present in 90% of ransomware breaches in 2023, making it a critical security risk.

## 2. How RDP is Abused by Attackers
Attackers primarily use exposed RDP services for initial access and lateral movement:  
1. **Initial Access**: Attackers connect to a publicly exposed RDP service and attempt to authenticate via Brute Force or by using valid, leaked credentials (Credential Stuffing).  
2. **Credential Dumping**: Once inside, the attacker may perform credential dumping to steal valid credentials from the compromised server.  
3. **Lateral Movement**: The stolen credentials are then used to move to other machines on the internal network via RDP, eventually reaching their objective (e.g., deploying ransomware or exfiltrating data).  

## 3. Identifying Exposed RDP Services
Publicly exposed RDP services can be located using specialized search engines:  
- **Shodan (shodan.io)**: Search using the query `port:3389` to find assets exposing the default RDP port.  
- **Censys (censys.com)**: Can be used to search for the port and filter specifically for the RDP service name.  
**Action Item**: Organizations should regularly use these tools to audit their own public IP space for unintentionally exposed services.  

## 4. Defense and Protection Recommendations
To protect against RDP abuse, implement security layers:  
1. **Turn Off RDP**: If the protocol is not actively required for operations, disable it entirely.  
2. **Multi-Factor Authentication (MFA)**: Always use MFA everywhere possible, providing an additional layer of protection against compromised passwords.  
3. **Restrict Access (Firewall)**: Implement strict firewall rules (or VPC rules) to:  
  - Restrict RDP access only to required, trusted source IP ranges.  
  - Put RDP behind a VPN (Virtual Private Network) so only users connected to the organization's VPN can reach the service.  
4. **Stronger Passwords**: Use 15+ character passwords containing complex characters. Utilize a Privileged Access Management (PAM) tool for one-time passwords if possible.  
5. **Disable/Rename Default Accounts**: Disable or rename default local administrator accounts (common brute force targets). *Note: Less effective against credential stuffing if attackers have valid credentials.*  
