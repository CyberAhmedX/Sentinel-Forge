# Day 16 Alerts and Dashboards (Windows & Advanced Rules)

## 1. Identify and Save the RDP Query (Kibana Discover)
1. **Filter for Source**: Navigate to Discover and filter for the Windows Server's agent name (e.g., `agent.name: win-target-server`).
2. **Identify Failed RDP**: Use the known Windows Event ID for failed logons: `event.code: 4625`.
3. **Refine Query**: Note that Logon Type 3 is typically network-based authentication, which includes RDP. Add the necessary fields as columns: `source.ip` and `user.name`.
4. **Save Search**: Save the completed query (e.g., "RDP failed Activity").

## 2. Create the RDP Threshold Alert (Standard Method)
This alert provides limited detail but confirms the query works.
1. **Start Rule Creation**: Click Alerts and select *Create search threshold rule*.
2. **Naming Convention**: Name the rule using your own choice name and handle (e.g., "RDP Brute Force Activity - [YourHandle]").
3. **Configure Threshold**: Set the condition to be triggered if the count of events where `event.code: 4625` is above 5 within the last 5 minutes.
4. **Save**: Click *Save Rule*.

## 3. Create Advanced Detection Rules (Security App)
Detection Rules provide rich context (Source IP, User, etc.) needed for investigation, unlike the standard Alerts above.

### Rule A: SSH Brute Force Attempt
1. **Navigate**: Go to Security → Rules → *Create new rule* → Threshold.
2. **Custom Query**: Enter the full KQL query for failed SSH logins, adding a filter for the user account (e.g., `user.name: root`).
3. **Group By (Crucial)**: Set the rule to group results by `source.ip` and `user.name`. This ensures the alert is specific to a unique attacker/user pair.
4. **Required Fields**: Add `source.ip` and `user.name` to the required fields list.
5. **Naming**: Name the rule using your own choice name and handle (e.g., "SSH Brute Force Attempt - [YourHandle]").
6. **Schedule**: Set to run every 5 minutes with a look back time of 5 minutes.
7. **Save**: Click *Create and enable rule*.

### Rule B: RDP Brute Force Attempt
1. **Action**: Click *Create new rule* → Threshold.
2. **Custom Query**: Enter the query for Windows failed logons (`event.code: 4625`) and filter for the Windows Server administrator account (e.g., `user.name: administrator`).
3. **Group By**: Set the rule to group results by `source.ip` and `user.name`.
4. **Schedule**: Set to run every 1 minute with a look back time of 5 minutes.
5. **Save**: Click *Create and enable rule*.

## 4. Alert Investigation (Verification)
- **Access Alerts**: View the generated alerts under Security → Alerts.
- **Review Details**: Open an RDP Brute Force alert to confirm it provides:
  - Source IP and Affected User directly in the alert summary.
  - Threshold Field/Value: Shows the exact Source IP and Username that triggered the threshold count.
- **Benefits**: This superior detail is provided because the rule was created via the Detection Rules interface, allowing for detailed aggregation and context compared to the standard "Alerts" tab feature.
