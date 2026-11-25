# T-Pot Honeypot Investigation: Cowrie & MiniPrint

## 1. Project Overview

This project investigates suspicious network activities captured through the T-Pot honeypot platform using the ELK Stack.

The investigation focuses on two honeypots:

- **Cowrie:** Monitors suspicious interactions and authentication attempts against SSH and Telnet services.
- **MiniPrint:** Monitors interactions with an exposed network printing service.

The collected logs were analysed using Kibana to identify attack patterns, suspicious source IP addresses, targeted ports and potential security threats.

## 2. Project Objectives

- Analyse security activities captured by Cowrie and MiniPrint.
- Identify common attack patterns.
- Examine suspicious source IP addresses.
- Investigate targeted network services and destination ports.
- Understand how attackers interact with exposed systems.
- Recommend security measures to improve network protection.

## 3. Tools and Technologies

| Tool | Purpose |
|---|---|
| T-Pot | Honeypot environment |
| Cowrie | SSH and Telnet activity monitoring |
| MiniPrint | Network printing service monitoring |
| Elasticsearch | Log indexing and storage |
| Logstash | Log processing |
| Kibana | Log analysis and visualization |
| KQL | Searching and filtering security logs |
| Talos Intelligence | IP intelligence reference |

## 4. Investigation Methodology

1. Monitored the T-Pot honeypot environment.
2. Accessed collected logs through Kibana.
3. Filtered records by honeypot type.
4. Examined usernames, passwords, source IPs, GeoIP locations and destination ports.
5. Analysed event types and activity timelines.
6. Interpreted the findings and developed security recommendations.

## 5. Investigation Findings

### A. Cowrie Honeypot Analysis

**Objective:** To analyse Cowrie logs, identify suspicious login attempts, examine common usernames, passwords and source IP addresses, and identify potential brute-force attacks targeting SSH and Telnet services.

#### Username Analysis

| Username | Percentage |
|---|---:|
| root | 44.3% |
| admin | 12.9% |

Other observed usernames included default, support, config, nobody and operator.

**Interpretation:**

The high occurrence of root and admin suggests attempts to use commonly known administrative accounts. Other usernames may indicate username enumeration or credential guessing.

Some records contained HTTP request headers and browser information rather than actual usernames. These mixed records should be validated before treating all entries as authentication attempts.

#### Password Analysis

| Password | Percentage |
|---|---:|
| admin | 10.3% |
| root | 10.3% |
| 123 | 7.4% |
| 1234 | 5.9% |
| 44444 | 2.9% |
| Blank values | 4.4% |

Other observed values included config00 and nobody0.

**Interpretation:**

The appearance of common usernames, simple numeric passwords and configuration-related values suggests possible password guessing or credential-testing activity.

#### Source IP Analysis

| Source IP | Percentage |
|---|---:|
| 138.197.147.76 | 8.0% |
| 121.190.92.220 | 6.5% |

The remaining 66.5% of recorded activity was distributed across other IP addresses.

**Interpretation:**

Multiple external IP addresses generated failed authentication events. The concentration around the leading IP addresses may indicate repeated credential-guessing attempts.

#### Geographical Analysis (GeoIP)

| Country | Percentage |
|---|---:|
| South Korea | 12.6% |
| United States | 11.9% |
| Canada | 10.4% |
| Netherlands | 8.8% |
| Other countries | 31.5% |

**Interpretation:**

The activity was associated with IP addresses mapped to different countries across Asia, North America and Europe, suggesting geographically distributed activity.

GeoIP data does not establish the attacker's actual physical location or identity.

#### Destination Port Analysis

| Port | Service | Percentage |
|---|---|---:|
| 23 | Telnet | 73.5% |
| 22 | SSH | 26.5% |

**Interpretation:**

Telnet received the highest proportion of observed activity, followed by SSH. Both services provide remote access and can be targeted through password-guessing attempts.

#### Cowrie Conclusion

The Cowrie investigation identified repeated failed authentication activity involving common administrative usernames, simple password values and multiple external source IP addresses.

The activity was predominantly directed at Telnet (port 23), followed by SSH (port 22). These findings are consistent with possible automated brute-force and credential-guessing attempts.

### B. MiniPrint Honeypot Analysis

**Objective:** To investigate activity recorded by the MiniPrint honeypot, identify patterns of interaction with the exposed printing service and interpret potential security risks.

#### Event Type Analysis

| Event Type | Percentage |
|---|---:|
| command_received | 27.3% |
| connection | 13.8% |
| save_raw_print_job | 13.6% |
| append_raw_print_job | 13.6% |
| response_sent | 13.6% |
| empty_response | 13.6% |
| connection_closed | 4.4% |

**Interpretation:**

The most frequent event was command_received at 27.3%, indicating that the honeypot received commands from external connections.

Other events involved print-job handling, service responses and connection management.

#### Source IP Analysis

| Source IP | Percentage |
|---|---:|
| 57.129.28.7 | 99.6% |
| 152.32.199.33 | 0.2% |

**Interpretation:**

Almost all recorded MiniPrint activity was concentrated around one source IP address, 57.129.28.7. This may suggest repeated interactions, automated probing or concentrated activity from that source.

#### GeoIP Country Analysis

| Country | Percentage |
|---|---:|
| Germany | 99.6% |
| Brazil | 0.2% |
| China | 0.2% |
| France | Negligible |
| United States | Negligible |

**Interpretation:**

Almost all MiniPrint activity was associated with IP addresses geographically mapped to Germany. This corresponds with the source IP analysis.

GeoIP information does not establish the attacker's actual physical location or identity.

#### Destination Port Analysis

The MiniPrint honeypot recorded 14,899 sample events, with 100% targeting destination port 9100.

Port 9100 is commonly used for network printing through the JetDirect/AppSocket printing protocol.

**Interpretation:**

All observed sample events targeted the network printing service monitored by the honeypot. This activity may represent automated probing or attempts to interact with an exposed printing service.

#### Activity Timeline

Key observations:

- **16–17 November 2026:** Very low activity.
- **18 November 2026:** Significant increase in recorded activity.
- **Peak interval:** 15:00–18:00, with 2,903 records.
- **19 November 2026:** Activity remained high in the early hours before declining.
- **20 November 2026:** Activity continued, with more than 1,000 records in each of the first four three-hour intervals shown.

**Interpretation:**

The timeline indicates a concentrated period of intense interaction with the MiniPrint honeypot, potentially involving automated activity.

#### MiniPrint Conclusion

The MiniPrint investigation identified concentrated activity against an exposed network printing service.

All 14,899 sample events in the destination-port analysis targeted port 9100. The majority of activity was associated with source IP 57.129.28.7, geographically mapped to Germany.

Repeated command submissions, print-job-related events and a significant increase in activity on 18 November suggest possible automated probing or repeated interactions with the exposed printing service.

## 6. Overall Investigation Conclusion

The investigation of the Cowrie and MiniPrint honeypots revealed different patterns of suspicious network activity.

- **Cowrie:** Recorded repeated failed authentication attempts targeting SSH and Telnet services, consistent with possible brute-force and credential-guessing attacks.
- **MiniPrint:** Recorded concentrated interactions with an exposed network printing service on port 9100, suggesting possible automated probing or repeated service interactions.

The findings demonstrate how exposed network services can attract suspicious activity and highlight the importance of strong authentication, access restrictions, continuous monitoring and proper network security practices.

## 7. Security Recommendations

1. Use strong and unique passwords.
2. Avoid default administrative credentials.
3. Disable unnecessary Telnet services and use secure alternatives such as SSH.
4. Implement firewall rules and access controls.
5. Monitor repeated failed login attempts and suspicious IP addresses.
6. Restrict public access to network printing services.
7. Restrict access to port 9100.
8. Monitor unusual print-job submissions.
9. Keep network services updated and properly secured.
10. Conduct regular security log reviews.

## 8. Skills Demonstrated

- Honeypot monitoring and investigation.
- ELK Stack log analysis.
- Kibana Discover and dashboard exploration.
- KQL search and filtering.
- SSH and Telnet authentication analysis.
- Source IP investigation.
- GeoIP analysis.
- Network port and service analysis.
- Event classification.
- Timeline analysis.
- Threat interpretation.
- Security reporting and documentation.

## 9. Project Limitations

- Findings represent activity captured during the monitoring period.
- IP geolocation does not prove the identity or physical location of an attacker.
- Some Cowrie username and password records contained HTTP-related information and require validation.
- Observed activity suggests possible attacks but does not prove that a real production system was compromised.

## 10. Project Evidence

Kibana screenshots and investigation report can be added to the repository.

Suggested folder structure:

tpot-honeypot-investigation/
- README.md
- screenshots/
- docs/

## Project Information

**Project:** T-Pot Honeypot Investigation  
**Platform:** T-Pot  
**Analysis Tool:** ELK Stack / Kibana  
**Honeypots:** Cowrie and MiniPrint  
**Focus:** Network Security Monitoring and Threat Analysis




⸻

Investigation Screenshots

1. T-Pot Dashboard

2. Cowrie Investigation

3. MiniPrint Investigation

























