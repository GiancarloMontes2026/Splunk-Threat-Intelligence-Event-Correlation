# Splunk Threat Intelligence & Event Correlation

## Overview

This project documents a hands-on Blue Team threat intelligence and SIEM investigation completed as part of CodePath's Intermediate Cybersecurity (CYB102) program.

The lab focused on using Splunk Enterprise to ingest external threat intelligence, correlate Indicators of Compromise (IOCs) with internal network activity, identify potentially suspicious behavior, and create dashboards and reports for continuous security monitoring.

The project demonstrates how Security Operations Center (SOC) analysts can combine threat intelligence with organizational log data to identify activity that may require further investigation.

## Objectives

The primary objectives of this lab were to:

- Import external threat intelligence into Splunk.
- Analyze threat intelligence data using Splunk Search & Reporting.
- Identify useful threat indicators such as IP addresses, URLs, and file hashes.
- Ingest network proxy logs into Splunk.
- Correlate threat intelligence with internal network activity.
- Use SPL searches to identify matching indicators across multiple data sources.
- Investigate suspicious TOR network activity.
- Identify affected systems and associated activity.
- Create a threat intelligence monitoring dashboard.
- Develop reports for communicating security findings.

## Lab Environment & Tools

The lab was performed in an Ubuntu virtual machine running Splunk Enterprise.

### Tools & Technologies

- Splunk Enterprise
- Splunk Search Processing Language (SPL)
- Threat Intelligence Feeds
- TOR IP Intelligence
- Network Proxy Logs
- Indicators of Compromise (IOCs)
- Ubuntu Linux
- SIEM
- Security Dashboards
- Security Reporting

## Threat Intelligence Ingestion

The investigation began by importing a static threat intelligence feed containing IP addresses associated with the TOR network into Splunk.

The CSV dataset was configured with the appropriate source type and host information before being indexed for analysis.

Once ingested, the threat intelligence became searchable within Splunk and could be used as a source of known indicators during security investigations.

## Threat Intelligence Analysis

After importing the threat feed, I explored the dataset to identify security-relevant fields such as:

- IP addresses
- URLs
- File hashes
- Threat indicators

This demonstrated how threat intelligence can provide additional context when analyzing activity occurring within an organization's environment.

## Network Log Correlation

A second dataset containing simulated network proxy activity was imported into Splunk.

The threat intelligence feed and network proxy logs shared a common IP address field, allowing the datasets to be correlated.

Using Splunk Search Processing Language (SPL), I created a search that compared IP addresses appearing in both datasets.

The investigation focused on identifying instances where an IP address associated with the TOR network also appeared within the organization's proxy logs.

## SPL Threat Correlation

The investigation used SPL to aggregate events from both sources and identify matching indicators.

Example:

(index=main source="TorList.csv") OR (index=main source="NetworkProxyLog01.csv")
| stats values(source) as sources,
        values("Computer Name") as ComputerName,
        values("User Agent String") as UserAgent,
        values(Date) as Date,
        values(Time) as Time
        by "IP Address"
| where mvcount(sources) > 1
| table "IP Address", ComputerName, UserAgent, Date, Time

This search correlates the threat intelligence feed with network proxy activity and displays IP addresses appearing across both sources.

Additional fields such as computer name, user agent, date, and time provided context for investigating the associated activity.

## Threat Investigation

After correlating the datasets, the results were analyzed to determine whether systems within the simulated environment had communicated with IP addresses associated with the TOR network.

This process demonstrated a fundamental SOC investigation workflow:

Threat Intelligence → Log Collection → IOC Correlation → Suspicious Activity Identification → Investigation

Rather than analyzing individual logs in isolation, multiple data sources were combined to provide greater context around potentially suspicious network behavior.

## Threat Monitoring Dashboard

After developing the correlation search, I created a Splunk dashboard titled:

**Threat Intelligence Monitoring**

The dashboard was configured to display instances where internal network activity matched indicators contained within the threat intelligence feed.

A statistics table was used to display relevant investigation fields such as:

- IP Address
- Computer Name
- User Agent
- Date
- Time

This demonstrated how threat intelligence searches can be converted into reusable monitoring capabilities for SOC analysts.

## Security Reporting

The dashboard search was also converted into a Splunk report.

Reports provide a way to preserve investigation queries and communicate security findings to other analysts and stakeholders.

In an enterprise environment, similar reports could support recurring threat monitoring, analyst investigations, and security operations workflows.

## Skills Demonstrated

Splunk Enterprise • SIEM • SPL • Threat Intelligence • Threat Hunting • IOC Analysis • Event Correlation • Log Analysis • Network Security Monitoring • TOR Analysis • Proxy Log Analysis • Security Dashboards • Security Reporting • SOC Operations • Blue Team Security

## Key Takeaways

This lab strengthened my understanding of how threat intelligence becomes more useful when correlated with internal security telemetry.

Rather than simply maintaining a list of known indicators, SIEM platforms such as Splunk can compare threat intelligence against organizational logs to identify activity that may require investigation.

The project followed a practical SOC workflow:

**Ingest Threat Intelligence → Collect Network Logs → Search & Correlate Events → Identify IOC Matches → Investigate Activity → Build Dashboard → Generate Report**

This project provided hands-on experience using Splunk to transform raw threat intelligence and network logs into actionable security information.

## Disclaimer

This project was completed in an authorized educational lab environment as part of CodePath's Intermediate Cybersecurity (CYB102) program. All threat intelligence, network logs, and investigative activities were used in a controlled environment for educational and defensive cybersecurity purposes.
