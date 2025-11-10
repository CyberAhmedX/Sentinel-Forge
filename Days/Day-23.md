# DAY 23: What is a Ticketing System?
This guide provides a high-level overview of ticketing systems and introduces the open-source solution that will be used in the challenge.

## 1. The Role of a Ticketing System
**Purpose:** A ticketing system is used to track alerts, tasks, and requests.  
**Objective:** The primary goal is to maintain a record of the task at hand, providing an audit trail and accountability.  
**Security Context:** This satisfies the "Accounting" or "Auditing" component of the AAA (Authentication, Authorization, and Accounting) security model.  
**Use Cases:** Tickets can be created for any type of request, including:  
- Security alerts  
- Customer complaints  
- Troubleshooting requests  

## 2. Common Ticketing System Software
**Commercial Products:** Popular software seen in enterprise environments includes:  
- Jira  
- ServiceNow  
- Freshdesk  
- Zendesk  
**Open-Source Solution:** The solution for this challenge is osTicket, an open-source product from Enhanced Soft.  

## 3. Overview: osTicket
**Key Features:** osTicket provides core functionalities found in commercial products, such as:  
- Customization of fields  
- Ticket filters for routing and assignment  
- Ability to assign and transfer tickets  
- Service Level Agreement (SLA) configuration  
**Deployment Options:**  
- **Self-Hosted (On-Prem):** This version is free and provides email integration, which is sufficient for this challenge.  
- **Cloud-Managed:** A paid option where osTicket manages the instance.
