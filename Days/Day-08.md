# Day 8 What is Sysmon?

## 1. Sysmon Fundamentals
- **Definition**: Sysmon (System Monitor) is a free utility from the Microsoft Sysinternals suite.
- **Purpose**: It fills the gap left by insufficient default Windows logging, particularly for forensic and incident response tasks. It is essential for catching malicious activity.
- **Customization**: Logging is highly flexible and requires a configuration file (recommended) to control which events are recorded, which helps reduce log noise.
- **Data Volume**: The current version offers 30 different event IDs to choose from.

## 2. Key Visibility Capabilities
Sysmon logs granular details necessary for threat hunting:
- **Process Creation**: Logs new processes, including the full command line of both the parent and child processes.
- **Hashing**: Records cryptographic hashes for process files, enabling OSINT lookups and verification.
- **Event Correlation**: Assigns a unique Process GUID to track activities related to a single process across multiple log events, which is critical for building a chain of events.
- **Network Connections (Event ID 3)**: Tracks all network activity, capturing source/destination IPs and ports along with the responsible process. This capability is disabled by default and must be enabled via the config file.

## 3. Critical Event IDs for Analysts
These event IDs are vital targets during a security investigation:
- **Event ID 1 (Process Creation)**: The primary log for tracking malware execution and command arguments. Includes the file hash.
- **Event ID 3 (Network Connections)**: Used to identify outbound Command and Control (C2) traffic and the originating executable. (Requires enabling).
- **Event IDs 6, 7, 8 (Driver/Image Load, Remote Thread)**: Used to detect advanced defense evasion techniques like process injection, where code is executed within another process. (Can be noisy).
- **Event ID 10 (Process Access)**: Important for finding attempts at credential access, particularly against the LSASS process.
- **Event ID 22 (DNS Query)**: Tracks all domain name resolution requests, useful for identifying communication with malicious or DGA-generated domains.
