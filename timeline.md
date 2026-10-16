
# Zeek Lab 1 — Investigation Timeline

## 1. Timeline Scope

This timeline records the observable events in the inspected Zeek connection and HTTP logs, followed by the analysis steps performed in the lab.

The timestamps below are copied from the logs. The original capture timestamps and the log-generation time are different; the latter reflects when Zeek processed the capture.

## 2. Network Activity Timeline

| Timestamp from Log | Source | Destination | Event | Evidence |
|---|---|---|---|---|
| `1747147647.668533` | `192.168.1.8:52917` | `192.0.78.212:80` | TCP connection recorded; service identified as HTTP | `conn.log` |
| `1747147647.702181` | `192.168.1.8:52917` | `192.0.78.212:80` | HTTP `GET /` for `zeek.org`; response status `301 Moved Permanently` | `http.log` |
| `1747147654.272660` | `192.168.1.8:52918` | `192.0.78.150:80` | TCP connection recorded; service identified as HTTP | `conn.log` |
| `1747147654.311012` | `192.168.1.8:52918` | `192.0.78.150:80` | HTTP request for `zeek.org`; method value `WEIRD`; response status `301 Moved Permanently` | `http.log` |

## 3. Connection Details

### Connection 1
- Connection UID: `CgLZKm2wmGoBzppupd`
- Source: `192.168.1.8:52917`
- Destination: `192.0.78.212:80`
- Protocol: TCP
- Service: HTTP
- Duration: approximately `0.098478` seconds
- Connection state: `SF`
- HTTP method: `GET`
- HTTP response: `301 Moved Permanently`

### Connection 2
- Connection UID: `CnGcJollwhglApons4`
- Source: `192.168.1.8:52918`
- Destination: `192.0.78.150:80`
- Protocol: TCP
- Service: HTTP
- Duration: approximately `0.100107` seconds
- Connection state: `SF`
- HTTP method value: `WEIRD`
- HTTP response: `301 Moved Permanently`

## 4. Analysis Activity Timeline

| Stage | Activity | Result |
|---|---|---|
| 1 | Verified Zeek installation | Version `8.0.10` confirmed |
| 2 | Copied the PCAP into the lab directory | `quickstart.pcap` available |
| 3 | Checked the capture format | PCAP 2.4, Ethernet |
| 4 | Executed Zeek against the capture | Log files generated |
| 5 | Inspected `conn.log` | Two connection records reviewed |
| 6 | Inspected `http.log` | Two HTTP records reviewed |
| 7 | Checked for DNS and TLS logs | `dns.log` and `ssl.log` were not generated |
| 8 | Identified an unusual HTTP method value | Further investigation required |
| 9 | Recorded evidence limitations | Investigation remains open |

## 5. Timeline Assessment

The inspected records show two short-lived TCP connections to HTTP services on port 80. Both corresponding HTTP transactions reference `zeek.org` and return status `301 Moved Permanently`.

The second transaction contains the method value `WEIRD`. The current timeline does not establish why this value appears or whether it represents malicious activity.

## 6. Outstanding Investigation Tasks

- [ ] Review the complete `weird.log`.
- [ ] Review the complete `files.log`.
- [ ] Review `packet_filter.log`.
- [ ] Examine all connection and HTTP records.
- [ ] Inspect relevant packets if the unusual method remains unexplained.
- [ ] Validate any identified indicators before making a security determination.

## Conclusion

The timeline establishes the order of the two inspected connection and HTTP events and records the principal analysis steps. Further log review and, if necessary, packet-level analysis are required before reaching a final security assessment.
