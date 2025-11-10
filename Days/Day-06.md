## Day 6 Elastic Agent and Fleet Server Intro

### 1. Elastic Agent
The Elastic Agent is the modern, unified tool for collecting data from endpoints.

- **Role**: Provides a single, unified method for monitoring logs, metrics, and other data types from a host.
- **Functionality**: Operates based on policies that are configured centrally to tell the endpoint exactly what data to forward to Elasticsearch or Logstash.
- **Installation Method**: The Fleet-Managed installation is being used for this challenge, allowing centralized control.
- **Agent vs. Beats**: The Agent is superior to the older Beats technology because it replaces the need to install multiple, specialized Beats (like Filebeat, Winlogbeat) with one single agent for all data collection.

### 2. Fleet Server
The Fleet Server is the central infrastructure component that enables agent management at scale.

- **Primary Role**: Acts as a connection point, linking all deployed Elastic Agents to the central Fleet application in Kibana.
- **Centralized Management Benefits**:
  - Allows for easy, instantaneous updates to agent policies across hundreds of endpoints (e.g., adding a new integration or changing the data output destination).
  - Simplifies agent lifecycle management, including version updates and agent unenrollment.
- **Necessity**: Without the Fleet Server, managing policies and updates would require painful, manual intervention on every single endpoint.
