# Day 2: Intro to ELK Stack

## ELK Components and Roles

1. The ELK stack consists of three distinct products:

   A. E - Elasticsearch (Database/Search Engine)

    - Function: Stores and indexes all log data (e.g., Windows events, Syslogs).

    - Access: Uses RESTful APIs and JSON.

    - Querying: Uses ESQL (Elasticsearch Query Language).

    - Splunk Analogy: Similar to the Indexer and Search Head.

   B. L - Logstash (Processing Pipeline)

    - Function: Collects, transforms, filters, and outputs data to Elasticsearch.

    - Crucial Tasks: Performs parsing (mapping log keywords to field names like src_ip) and log filtering to reduce ingestion costs.

    - Splunk Analogy: Similar to the Heavy Forwarder.

   C. K - Kibana (User Interface/Visualization)

    - Function: Provides the web console for analysts.

    - Tools: Includes the Discover tab for querying, Kibana Lens for creating dashboards, and integrated alerting and machine learning features.

    - Splunk Analogy: Similar to the Web GUI.

## Data Collection

1. Role: Dedicated agents are installed on endpoints to ship logs.

2. Methods: Uses the Elastic Agent (a unified agent used in the challenge) or individual, lightweight Beats (e.g., Winlogbeat, Filebeat).

3. Splunk Analogy: Similar to the Universal Forwarder.

# Key Benefits

1. Centralized Logging: Essential for meeting compliance and speeding up incident response.

2. Flexibility: Allows precise control over what data is ingested.

3. Scalability: The infrastructure is designed to be easily expanded.

4. Visualization: Ability to create high-value, informative dashboards.

5. Ecosystem: Experience is transferable to many commercial SIEM (Security Information and Event Management) platforms.
