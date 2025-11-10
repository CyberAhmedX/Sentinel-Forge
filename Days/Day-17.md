# Day 17 Alerts and Dashboards (RDP Visualization)

## 1. Create the RDP Visualization Map (Kibana Maps)
1. **Access Maps**: Navigate to Analytics → Maps.  
2. **Input Query**: Enter the full KQL query for failed RDP logons (the query saved previously that includes `event.code: 4625` and the Windows server agent name).  
3. **Add Layer**: Select **Add Layer** and choose the **Choropleth layer** type.  
4. **Configure Geo-Mapping**:  
   o **Boundary Source**: Select the EMS boundaries → World Countries.  
   o **Statistic Source**: Use the index containing your Windows logs.  
   o **Join Field**: Set the join field to use the source country's abbreviated ISO code: `source.geo.iso_code`.  
5. **Save Visualization**: Save the completed map visualization (e.g., `RDP Failed Network Map`).  

## 2. Configure the Final Dashboard
1. **Create Dashboard**: Create a new dashboard (or open the existing authentication dashboard created in Day 14).  
2. **Add RDP Map**: Add the newly saved `RDP Failed Network Map` visualization to the dashboard.  
3. **Add RDP Failed Search**: Add the saved search for `RDP failed Activity` to the dashboard for detailed event viewing.  
4. **Optional: Successful Logins**:  
   o Duplicate the `RDP Failed Search` visualization.  
   o **Edit the Query**: Change the query filter from `event.code: 4625` (failed) to the Windows successful logon event code: `event.code: 4624`.  
   o **Save**: Save this new search (e.g., `RDP Successful Logins`) and add it to the dashboard.  
5. **Save Final Dashboard**: Set the title to your own choice name (e.g., `Windows Authentication Activity Dashboard`) and save the final layout.  

