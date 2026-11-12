
## Evidence Privacy and Anonymization

This project uses real network traffic captured in a controlled home-network environment.

To protect device and network privacy:
- Private device IP addresses in published reports were replaced with documentation-only IP addresses from the 192.0.2.0/24 range.
- A device-specific MAC address was replaced with a locally administered example MAC address.
- The original LAN broadcast destination was represented using 192.0.2.255. This is a documentation placeholder, not an actual observed broadcast address.
- Original PCAP captures and Suricata logs are retained locally and excluded from Git tracking.
- Publicly documented findings preserve the relevant protocols, ports, alert classifications, and investigation conclusions.

All anonymized addresses are illustrative and should not be interpreted as the original observed network identifiers.
