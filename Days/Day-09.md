# Day 9 Sysmon Setup Tutorial

## 1. Preparation: Download Files
- **Access Windows Server**: Connect to the Windows Server VM via RDP.
- **Download Sysmon Binary**: Use a browser (e.g., Edge) to search for and download the official Sysmon executable package from the Microsoft Learn/Sysinternals website.
- **Download Configuration File**: Search for `Sysmon Olaf configuration` and navigate to the GitHub repository.
  - *Action*: Select the `sysmonconfig.xml` file, view the Raw content, and use Save As to download the file into the extracted Sysmon directory.

## 2. Installation
- **Access PowerShell**: Open PowerShell as Administrator on the Windows Server.
- **Navigate to Directory**: Change the directory to where the Sysmon files (`Sysmon64.exe` and `sysmonconfig.xml`) were extracted.
  ```powershell
  cd 
  ```
- **Install Sysmon**: Run the executable with the install flag (`-i`) and point it to the configuration file:
  ```powershell
  .\Sysmon64.exe -i sysmonconfig.xml
  ```
- **Action**: Agree to the license prompt to complete the installation.

## 3. Verification
- **Check Service Status**: Open the Windows Services application and verify that the `Sysmon64` service is listed and set to **Running**.
- **Check Event Logs**: Open the Windows Event Viewer.
  - Navigate to `Applications and Services Logs` → `Microsoft` → `Windows` → `Sysmon` → `Operational`.
  - *Verification*: Confirm that the log viewer is populating with new events, such as **Event ID 3 (Network Connections)**, which is enabled by the Olaf configuration.


