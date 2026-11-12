# Investigation 01: DNS Traffic Analysis

## Objective
Examine DNS requests and responses in a baseline network capture.

## Environment
- Operating system: macOS
- Capture tool: Wireshark 4.6.9
- Interface: en0 (Wi-Fi)
- Capture file: 01-baseline-network-traffic.pcapng

## Evidence
- Total captured packets: 4,014
- DNS packets: 88
- Domain investigated: example.com
- Query packet: 761
- Response packet: 770
- Transaction ID: 0x9437
- Record type: A
- Response code: No error
- Returned IPv4 addresses:
  - 104.20.23.154
  - 172.66.147.243
- TTL: 45 seconds
- DNS response time: 34.77 milliseconds

## Analysis
The DNS resolver returned two IPv4 addresses for example.com.
The query and response shared the same transaction ID,
and the response completed without a DNS error.

## Security Assessment
The observed exchange is consistent with normal DNS resolution.
No suspicious behavior was identified in this specific exchange.
Additional traffic analysis is required before drawing broader
security conclusions.
