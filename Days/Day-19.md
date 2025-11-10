# Day 19 How To Create an Attack Diagram

## 1. Attack Infrastructure Components
- **Attacker Host**: A machine running Kali Linux (Visualized in red).
- **Target Machine**: The Windows Target Server (Primary focus).
- **C2 Server**: A Command and Control (C2) Framework (Visualized in red, hosted in the cloud).
- **Network**: The Internet.

## 2. The Attack Diagram Phases
The diagram visually maps the execution chain, detailing the six planned phases:
- **Phase 1: Initial Access**
    - **Goal**: Gain initial entry to the Target Machine.
    - **Action**: Perform RDP Brute Force from the Attacker Host via the Internet.
- **Phase 2: Discovery**
    - **Goal**: Determine the attacker's current privileges and network context after successful login.
    - **Action**: Run basic Discovery Commands: `whoami`, `ipconfig`, `net user`, `net group`.
- **Phase 3: Defense Evasion**
    - **Goal**: Neutralize security software before deploying the payload.
    - **Action**: Use the established RDP session to disable the Windows Defender application on the Target Machine.
- **Phase 4: Execution**
    - **Goal**: Download and run the C2 agent payload.
    - **Action**: Use Powershell IEX (Invoke-Expression) to download the agent payload from the C2 Framework.
- **Phase 5: Command and Control (C2)**
    - **Goal**: Establish persistent, remote communication with the compromised host.
    - **Action**: Successfully establish the C2 session between the Target Machine and the C2 Framework.
- **Phase 6: Exfiltration**
    - **Goal**: Achieve the final objective of stealing data.
    - **Action**: Use the established C2 session to download a fake password file (`passwords.txt`) from the Target Machine to the C2 Server.

