# Day 5 : Windows server 2022

## 1. Network Diagram Update (Important Change)
- **Rationale**: The Windows Server is being deployed outside the Virtual Private Cloud (VPC) for security reasons. If the server is compromised, the attacker will not gain immediate access to the internal network containing the Fleet server and OS Ticket server.
- **Action**: Update the logical diagram (from Day 1) to remove the Windows Server (and later the Ubuntu Server) from the VPC boundary.
- **VPC Status**: The VPC will now only contain the Elastic/Kibana, Fleet Server, and OS Ticket server.

## 2. Vultr Windows Server Deployment
- **Access Vultr**: Log in and select Deploy New Server.
- **Type**: Select Cloud Compute (Shared CPU).
- **Location**: Choose the same Location as the other resources (e.g., Tokyo).
- **Image**: Select Windows Standard Windows Server 2022.
- **Size**: Select the budget option (e.g., $24/month plan: 1 vCPU, 2 GB RAM).
- **Configuration**:
  - Auto Backups/IPv6: Do not select these options.
  - Virtual Private Cloud: DO NOT select or attach the server to the VPC (as per the diagram change).
  - Firewall Group: Leave blank/default (The goal is to expose RDP to the internet).
- Name the Hostname

## 3. Server Access and Verification
1. **Wait**: Wait several minutes for the server status to change to Running and for the operating system to finish deploying.
2. **Access Console**: Select View Console in the Vultr UI.
3. **Log In**:
   - Click Send Control Alt Delete to bring up the login screen.
   - Copy the Password from the Vultr server details page.
   - Paste the password into the console and sign in.
4. **Verify RDP Exposure (Target Status)**:
   - Action: Copy the server's Public IP Address.
   - Open your local Remote Desktop Connection client.
   - Attempt to connect to the Public IP.
   - Verification: A successful connection proves RDP is exposed to the internet, allowing the server to generate logs (failed login attempts) for future analysis.
