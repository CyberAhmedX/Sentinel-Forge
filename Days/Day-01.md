# Day 1 : Logical Diagram

## Initial Setup and Environment Containers
1. Access Tool: Navigate to draw.io.

2. Name Diagram: Rename the file (e.g., 30-day my defer diagram).

3. Define Cloud: Use a rounded rectangle shape, label it Vultr, and send it To Back so icons sit on top.

4. Add Network: Search for and add the VPC (Virtual Private Cloud) icon, expanding it to enclose the server space.

## Adding and Labeling Core Servers
1. Add Servers: Search for "server" icon and duplicate it to create six instances.

2. Label Roles: Assign the specific roles to each server:

- Elastic and Kibana  
- Windows Server (with RDP)  
- Ubuntu Server (with SSH)  
- Fleet Server  
- OS Ticket Server  
- C2 (Command and Control) Server  

## Network Details and Connectivity
1. Define IP Range: Add text inside the VPC defining the private network range, e.g., 172.31.0.0/24 with the subnet mask.

2. Connect Logs: Draw arrows to show log forwarding:

3. Windows and Ubuntu → Elastic/Kibana (Label: forward logs via agent).

4. Connect Management: Draw arrows to show agent and ticket management:

5. Windows and Ubuntu → Fleet Server (Label: managed).

6. Fleet Server ➙ Elastic/Kibana (Use bidirectional arrow, Label: managed agents).

7. OS Ticket ➙ Elastic/Kibana (Use bidirectional arrow, Label: alerts/tickets).

8. Straighten Lines: Use the Waypoint tool to select Straight lines for a cleaner diagram.

## External Access and Attacker Elements

1. Add Internet Path: Add the Internet Gateway and Cloud (representing the Internet).

2. Connect Gateway: Connect the Internet → Internet Gateway → VPC.

3. Add Clients: Add the Laptop icon for the SOC Analyst Laptop.

4. Add Attacker: Duplicate the laptop, label it Attacker Laptop (representing Kali Linux).

5. Connect Clients: Connect both the SOC Analyst and Attacker Laptops to the Internet.

## Logical Diagram

https://github.com/CyberAhmedX/Sentinel-Forge/blob/main/Assets/Images/Logical-Network-Diagram.png

