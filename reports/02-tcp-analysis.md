# Investigation 02: TCP Connection and Retransmission Analysis

## Objective
Investigate TCP connection establishment, retransmissions,
and subsequent encrypted communication.

## Environment
- Operating system: macOS
- Analysis tool: Wireshark 4.6.9
- Capture file: 01-baseline-network-traffic.pcapng
- Interface: en0 (Wi-Fi)

## Connection Details
- TCP stream: 19
- Source: 192.0.2.44:49656
- Destination: 17.33.195.5:443
- Stream packet count: 101

## Evidence
- Packet 434: Initial SYN at 8.783835 seconds
- Packet 436: SYN retransmission at 8.895074 seconds
- Packet 437: SYN retransmission at 9.008432 seconds
- Packet 440: SYN-ACK at 9.054174 seconds
- Packet 442: Additional SYN-ACK at 9.054178 seconds
- Packet 445: Client ACK at 9.055930 seconds
- Packet 448: TLS Client Hello
- Packet 456: TLS Server Hello

## Analysis
The client initiated a TCP connection and transmitted two
additional SYN packets before receiving a SYN-ACK.

The client subsequently acknowledged the server response,
completing the TCP three-way handshake.

Observed handshake completion interval: 272.095 ms.

TLS negotiation and encrypted application traffic followed.

Additional TCP analysis indicators included duplicate ACKs
and a retransmission at packet 488.

## Security Assessment
The connection was successfully established.

The retransmissions indicate an observable network anomaly,
but their underlying cause cannot be determined from the
available evidence alone.

No malicious activity was established in this investigation.

## Conclusion
This investigation demonstrates TCP handshake analysis,
retransmission identification, and evidence-based
interpretation of network behavior.
