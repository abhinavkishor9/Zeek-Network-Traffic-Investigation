
# Zeek Lab 1 — Network Traffic Investigation

## 1. Environment
- Tool: Zeek 8.0.10
- Analysis mode: Offline PCAP
- Capture filename: `quickstart.pcap`
- Capture format: PCAP 2.4, Ethernet
- Operating system: Ubuntu 24.04 on WSL
- Working directory: `~/zeek-labs/lab01`

## 2. Evidence Reviewed
- Connection log: `conn.log` — two records inspected
- DNS log: `dns.log` — not generated during the initial run
- HTTP log: `http.log` — two records inspected
- TLS log: `ssl.log` — not generated during the initial run
- Other relevant logs:
  - `files.log` — generated; contents pending review
  - `weird.log` — generated; contents pending review
  - `packet_filter.log` — generated; contents pending review

## 3. Findings
- Communicating hosts:
  - Source: `192.168.1.8`
  - Destination: `192.0.78.212:80`
  - Destination: `192.0.78.150:80`
- Observed protocols and ports:
  - Transport protocol: TCP
  - Destination port: 80
  - Zeek identified the service as HTTP
  - Both inspected connections have state `SF`
- DNS activity:
  - No `dns.log` was generated.
  - DNS activity has not been established from the logs reviewed.
- HTTP/TLS activity:
  - Both HTTP records reference host `zeek.org`.
  - Both records contain URI `/`.
  - User agent: `curl/8.7.1`
  - Both responses returned status `301 Moved Permanently`.
  - One request used method `GET`.
  - The other request contains method value `WEIRD`.
  - No TLS activity has been established from the available logs.
- Unusual connection patterns:
  - Two short-lived HTTP connections were observed to different destination IP addresses.
  - The unusual HTTP method value warrants investigation.
  - The method value in `http.log` should not automatically be interpreted as a corresponding event in Zeek's `weird.log`.
  - The observations do not independently establish malicious activity.

## 4. Assessment
- Confirmed observations:
  - Zeek successfully processed `quickstart.pcap`.
  - Two TCP connection records from `192.168.1.8` to destination port 80 were inspected.
  - Both HTTP records reference `zeek.org` and report status `301`.
  - One HTTP record contains the method value `WEIRD`.
- Potential security concerns:
  - The unusual HTTP method may reflect malformed, non-standard, or otherwise unexpected HTTP traffic.
  - Its cause and security significance remain undetermined.
- Evidence limitations:
  - Only two connection records and two HTTP records have been inspected.
  - The complete contents of the generated logs have not been reviewed.
  - DNS and TLS activity have not been established from the generated logs.
  - The redirect destinations and the broader context of the traffic have not been independently validated.
  - Packet-level validation and threat-intelligence correlation have not been completed.
- Recommended follow-up:
  - Review `weird.log` for protocol anomalies and their associated context.
  - Review `files.log` for recorded file-related activity.
  - Examine `packet_filter.log` to understand the capture-processing information it records.
  - Review the complete `conn.log` and `http.log` files.
  - Inspect relevant packets to understand the unusual method and HTTP redirects if needed.
  - Correlate any resulting indicators with reliable threat-intelligence sources before drawing conclusions.

## Conclusion

The inspected Zeek logs establish two TCP connections from `192.168.1.8` to `192.0.78.212` and `192.0.78.150` on destination port 80. Both corresponding HTTP records reference `zeek.org` and report `301 Moved Permanently`. One request contains the unusual method value `WEIRD`.

The available evidence is insufficient to classify the traffic as malicious or confirm a security incident. The investigation remains open pending review of the remaining logs and any relevant packet-level evidence.
