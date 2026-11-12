
# Suricata IDS Alert Investigation Report

## Project Information

- Project: Network Intrusion Detection Lab
- Investigation: Baseline Traffic Analysis
- Date: November 12, 2026
- Analyst Role: SOC Analyst
- Tools: Wireshark, Suricata 8.0.8, macOS Terminal
- Capture: `captures/01-baseline-network-traffic.pcapng`
- Alert Log: `logs/baseline/fast.log`
- Structured Events: `logs/baseline/eve.json`

## 1. Executive Summary

This investigation analyzed baseline network traffic using Suricata, an open-source intrusion detection system (IDS), and Wireshark.

The objective was to identify security alerts, correlate detections with captured network packets, and distinguish potentially malicious activity from normal network behavior.

Suricata processed 4,014 captured packets and generated 49 alerts across three detection signatures.

### Detection Summary

| Detection Signature | Alerts | Classification |
|---|---:|---|
| SURICATA Ethertype unknown | 45 | Generic Protocol Command Decode |
| ET INFO TLS Handshake Failure | 3 | Potentially Bad Traffic |
| ET INFO Spotify P2P Client | 1 | Not Suspicious Traffic |
| **Total** | **49** | |

No confirmed intrusion was identified during this baseline investigation.

## 2. Investigation Environment

The analysis was performed on a MacBook using Wireshark and Suricata.

Suricata was configured to process an existing packet capture rather than monitor live network traffic.

The detection engine used the installed Suricata ruleset, which contained 53,291 lines beginning with an active `alert` action.

### Analysis Workflow

1. Capture baseline network traffic using Wireshark.
2. Save the original PCAPNG evidence.
3. Process the capture with Suricata.
4. Generate structured event logs and security alerts.
5. Group alerts by detection signature.
6. Investigate representative packets in Wireshark.
7. Classify the observed behavior.
8. Document the evidence and conclusions.

## 3. Investigation A: Unknown Ethertype Alerts

### Detection Information

- Signature: SURICATA Ethertype unknown
- Signature ID: 2200121
- Alerts: 45
- Priority: 3
- Classification: Generic Protocol Command Decode

### Packet Evidence

Representative packets examined:

- Packet 82
- Packet 126
- Packet 404

Packets 126 and 404 contained Realtek Layer 2 Protocol traffic.

Observed properties included:

- Destination MAC: FF:FF:FF:FF:FF:FF
- EtherType: 0x8899
- Protocol: Realtek Layer 2 Protocols
- Source MAC: 02:00:00:00:00:01

Packet 82 contained an IEEE 802.3 LLC/XID frame with a broadcast destination.

### Technical Analysis

Suricata identified certain link-layer frames as having unknown or unsupported EtherTypes.

Wireshark identified representative frames as Realtek Layer 2 Protocol and LLC/XID traffic.

These detections illustrate differences in protocol recognition between packet-analysis tools.

An unknown-Ethertype alert does not independently establish malicious activity.

### Security Assessment

- Finding: Unrecognized link-layer traffic
- Evidence: Broadcast Ethernet frames and protocol decoding
- Threat determination: No confirmed malicious behavior
- Disposition: Likely benign or environment-specific traffic

### Recommendation

Document the observed link-layer protocols as part of the network baseline.

Investigate further if these alerts become unusually frequent, originate from unexpected devices, or accompany other suspicious activity.

Avoid suppressing the detection rule without evaluating its relevance to the monitored environment.

## 4. Investigation B: TLS Handshake Failures

### Detection Information

- Signature: ET INFO TLS Handshake Failure
- Signature ID: 2029340
- Alerts: 3
- Priority: 2
- Classification: Potentially Bad Traffic

### Network Endpoints

- Local IP: 192.0.2.44
- Remote IP: 104.20.23.154
- Remote Port: TCP 443

### Alert Evidence

| Local Port | Wireshark Alert Packet | Result |
|---|---:|---|
| 49658 | 829 | Fatal Handshake Failure |
| 49660 | 1479 | Fatal Handshake Failure |
| 49662 | 1808 | Fatal Handshake Failure |

All three connections followed a similar sequence:

1. Local host initiated a TCP connection.
2. Remote server acknowledged the connection.
3. TCP three-way handshake completed.
4. Local host transmitted a TLS Client Hello.
5. Remote server returned a fatal TLS alert.
6. The connection was subsequently closed.

### TLS Alert Details

- Alert Level: Fatal (2)
- Alert Description: Handshake Failure (40)
- Alert Sender: 104.20.23.154
- Transport Protocol: TCP

The third connection also exhibited TCP retransmissions.

### Technical Analysis

The successful TCP handshakes demonstrate that transport-layer connectivity was established.

However, the TLS negotiation did not complete successfully.

Possible explanations include incompatible TLS parameters, server-side restrictions, or other handshake negotiation issues.

The available evidence does not identify the precise underlying cause.

TCP retransmissions were also observed but do not independently establish an attack.

The TLS record-layer legacy version field should not be interpreted as proof of the negotiated TLS protocol version.

### Security Assessment

- Finding: Three confirmed TLS handshake failures
- Detection accuracy: Supported by packet evidence
- Threat determination: No confirmed malicious behavior
- Root cause: Undetermined
- Disposition: Likely benign, pending additional context

### Recommendation

Monitor for repeated TLS failures involving unexpected destinations, abnormal connection volumes, or suspicious processes.

If the failures persist, inspect the TLS Client Hello parameters, application logs, and endpoint behavior.

## 5. Investigation C: Spotify P2P Client Alert

### Detection Information

- Signature: ET INFO Spotify P2P Client
- Signature ID: 2027397
- Alerts: 1
- Priority: 3
- Classification: Not Suspicious Traffic

### Network Evidence

- Source IP: 192.0.2.34
- Destination IP: 192.0.2.255
- Source Port: UDP 57621
- Destination Port: UDP 57621
- Representative Wireshark Packet: 2374
- Packet Size: 86 bytes
- UDP Payload: 44 bytes
- Destination MAC: FF:FF:FF:FF:FF:FF

### Technical Analysis

The packet originated from a private IPv4 address and was sent to a local broadcast destination.

Suricata identified the traffic using its Spotify P2P Client signature.

Wireshark confirmed the UDP broadcast communication and displayed the application payload.

However, Wireshark did not independently identify the application as Spotify.

The Spotify attribution therefore remains based on the Suricata signature.

The packet's IP addresses and ports match the alert log. The exact packet-number association was not independently established from the structured event record.

### Security Assessment

- Finding: Local UDP broadcast matching a Spotify-related signature
- Classification: Not Suspicious Traffic
- Threat determination: No confirmed malicious behavior
- Disposition: Likely benign informational event

### Recommendation

Retain this traffic pattern in the baseline documentation.

Investigate only if similar broadcasts occur at abnormal rates, originate from unexpected devices, or correlate with additional suspicious indicators.

## 6. Consolidated Security Assessment

| Detection | Count | Assessment |
|---|---:|---|
| Unknown Ethertype | 45 | Likely benign link-layer activity |
| TLS Handshake Failure | 3 | Confirmed failures; cause undetermined |
| Spotify P2P Client | 1 | Likely benign informational activity |

### Key Findings

1. Suricata successfully processed the saved network capture.
2. The installed detection rules generated 49 alerts.
3. Wireshark provided packet-level evidence supporting the investigated detections.
4. Three TLS connections failed during negotiation.
5. Local broadcast traffic triggered informational protocol detections.
6. No confirmed intrusion was established from the available evidence.

### Limitations

This analysis covered a limited baseline capture rather than continuous network monitoring.

A lack of confirmed malicious activity does not establish that the network is free from compromise.

Alert classification relied on the available packet evidence and Suricata detection metadata.

Additional endpoint logs, process information, and historical traffic would improve confidence in the findings.

## 7. SOC Analyst Skills Demonstrated

This investigation demonstrates practical experience with:

- Network packet analysis
- IDS alert triage
- Suricata signature interpretation
- TCP connection analysis
- TLS handshake investigation
- UDP broadcast analysis
- Protocol identification
- False-positive and benign-event assessment
- Evidence correlation
- Security incident documentation

## 8. Conclusion

The baseline intrusion detection investigation successfully correlated Suricata security alerts with Wireshark packet evidence.

A total of 49 alerts were identified across three signatures.

The investigation distinguished valid detection events from confirmed security incidents and documented the observed network behavior.

No confirmed malicious activity was identified.

The findings establish a reference baseline for future intrusion detection testing, custom rule development, and controlled security simulations.

