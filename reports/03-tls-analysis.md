# Investigation 03: TLS Encryption Analysis

## Objective
Investigate TLS handshake negotiation and identify
the cryptographic parameters used by a network connection.

## Environment
- Operating system: macOS (Apple Silicon)
- Analysis tool: Wireshark 4.6.9
- Capture: 01-baseline-network-traffic.pcapng
- Interface: en0 (Wi-Fi)

## Connection Details
- TCP stream: 6
- Client: 192.0.2.44:49654
- Server: 17.171.47.23:443
- SNI: gdmf.apple.com

## Evidence
- Packet 132: TLS Client Hello
- Packet 140: TLS Server Hello
- Negotiated TLS version: TLS 1.3
- Cipher suite: TLS_AES_256_GCM_SHA384
- Key share: X25519MLKEM768
- Server Hello timestamp: 1.757194 seconds

## Technical Analysis
The client advertised support for TLS 1.3 and TLS 1.2.

The server selected TLS 1.3 through the supported_versions
extension.

The selected cipher suite was TLS_AES_256_GCM_SHA384.

The server selected X25519MLKEM768, a hybrid
classical/post-quantum key-establishment mechanism.

The legacy TLS version fields do not represent the
negotiated protocol version.

## Security Assessment
The observed connection negotiated modern encryption
parameters.

TLS protects application content, although connection
metadata may remain observable to network monitoring tools.

No malicious activity was established from this handshake.

## Conclusion
This investigation demonstrates TLS handshake analysis,
cryptographic parameter identification, and encrypted
traffic interpretation.
