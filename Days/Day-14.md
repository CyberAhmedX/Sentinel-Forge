# Day 14: How To Create Alerts and Dashboards in Kibana

## 1. Identify and Save the Query (Kibana Discover)
1. **Filter for Source**: Navigate to Discover and filter for the Linux server's agent name (e.g., `agent.name: ssh-server-linux-policy`).
2. **Identify Failed Attempts**: Determine the correct field to identify authentication activity (e.g., `system.auth.ssh.event`).
3. **Refine Query**: Filter the events to include only failed attempts where the authentication event field exists, and include key fields as columns: 
   - `user.name`
   - `source.ip`
   - `source.geo.country_name`
4. **Save Search**: Save the completed query for reuse (e.g., `SSH failed activity`).

## 2. Create the Brute Force Alert (Kibana Alerting)
1. **Start Rule Creation**: Click the Alerts tab (within the Discover view) and select *Create search threshold Rule*.
2. **Naming Convention**: Name the rule using your own choice name and handle (e.g., `SSH Brute Force Activity - [YourHandle]`).
3. **Configure Threshold**: Set the rule to monitor the number of failed events.  
   - *Condition*: Set is above a low number (e.g., `5`) for the time duration.  
   - *Look back*: Set the time window to a short duration (e.g., `last 5 minutes`).  
4. **Schedule**: Set the rule to check for this condition periodically (e.g., `every 1 minute`).
5. **Save**: Click *Save Rule*.

## 3. Build the Visualization Dashboard
1. **Access Maps**: Navigate to Analytics → Maps.
2. **Input Query**: Enter the saved brute force query into the search bar.
3. **Add Visualization Layer**: Click *Add Layer* and select the *Choropleth* layer.
4. **Configure Geo-Mapping**:  
   - *Boundary Source*: Select the EMS boundaries → World Countries.  
   - *Statistic Source*: Use the index where your Linux logs are stored.  
   - *Join Field*: Set the join field to use the source country's abbreviated ISO code (e.g., `source.geo.iso_code`) to map against the world country boundaries.  
5. **Save Visualization**: Save the map visualization (e.g., `SSH Network map`) and add it to a new dashboard.
6. **Final Dashboard**: Rename the dashboard (e.g., `Authentication Activity`). You can optionally duplicate the map and change the query to filter for accepted events to track successful logons on the same dashboard.
7. **Save Dashboard**: Save the completed dashboard.
