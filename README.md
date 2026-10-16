# Zeek Lab 1 — Network Traffic Investigation

## Overview

This lab introduces network traffic investigation using Zeek, an open-source network security monitoring tool. The investigation uses an offline PCAP capture to examine connection metadata, HTTP transactions, and potentially unusual protocol behavior.

The objective is to understand how network evidence can be extracted, correlated, and documented without automatically treating unusual activity as malicious.

## Lab Objectives

- Install and verify Zeek in Ubuntu 24.04 on WSL.
- Process an offline PCAP file using Zeek.
- Interpret connection metadata in `conn.log`.
- Analyze HTTP transactions in `http.log`.
- Understand connection states, source and destination endpoints, ports, HTTP status codes, and user-agent values.
- Identify unusual observations that require further investigation.
- Document evidence limitations and recommend appropriate follow-up actions.

## Lab Environment

| Component | Details |
|---|---|
| Operating system | Ubuntu 24.04 on WSL |
| Network analysis tool | Zeek 8.0.10 |
| Analysis mode | Offline PCAP analysis |
| Capture file | `quickstart.pcap` |
| Capture format | PCAP 2.4, Ethernet |
| Analysis directory | `~/zeek-labs/lab01` |

## Investigation Scenario

A small packet capture is provided for an initial network traffic investigation. The analyst must process the capture with Zeek, review the generated logs, identify communicating hosts and observed protocols, and determine whether any activity requires additional examination.

The initial analysis identifies two TCP connections to destination port 80. Both associated HTTP records reference `zeek.org` and return an HTTP status of `301 Moved Permanently`. One HTTP record contains the method value `WEIRD` instead of the `GET` method observed in the other record.

The unusual method value is an observation to investigate, not proof of malicious behavior. Its significance must be assessed using the relevant logs and, if necessary, packet-level evidence.


## Lab Workflow

### 1. Verify Zeek

```bash
/opt/zeek/bin/zeek --version
```

Expected version:

```text
zeek version 8.0.10
```

### 2. Prepare the Lab Directory

```bash
mkdir -p ~/zeek-labs/lab01
cd ~/zeek-labs/lab01
```

Copy the supplied capture into the directory:

```bash
cp "/mnt/d/important docs/Cybersecurity labs and resources/quickstart.pcap" .
```

Verify the capture file:

```bash
ls -lh quickstart.pcap
file quickstart.pcap
```

### 3. Process the Capture

```bash
/opt/zeek/bin/zeek -r quickstart.pcap
```

Zeek processes the capture offline and writes its logs to the current working directory.

List the generated logs:

```bash
ls -lh *.log
```

### 4. Review Connection Records

```bash
cat conn.log
```

Inspect the connection endpoints, transport protocol, destination ports, identified services, connection durations, byte counts, and connection states.

### 5. Review HTTP Transactions

```bash
cat http.log
```

Examine the HTTP method, host, URI, user agent, response status, and other available transaction fields.

### 6. Check Other Logs

```bash
cat weird.log
cat files.log
cat packet_filter.log
```

Review each log before drawing conclusions. The presence of a generated log file does not establish that it contains a security incident.

## Initial Findings

- Source host observed: `192.168.1.8`
- Destination endpoints observed: `192.0.78.212:80` and `192.0.78.150:80`
- Transport protocol: TCP
- Identified service: HTTP
- HTTP host field: `zeek.org`
- URI observed: `/`
- User agent: `curl/8.7.1`
- HTTP response status: `301 Moved Permanently`
- Unusual HTTP method value: `WEIRD` in one transaction
- Connection state: `SF` for both inspected connection records

These findings describe the inspected records only. A complete assessment requires reviewing the remaining relevant evidence.

## Evidence Limitations

At the time of the initial review:

- Two connection records and two HTTP records had been inspected.
- `dns.log` and `ssl.log` were not generated.
- The contents of `weird.log`, `files.log`, and `packet_filter.log` had not yet been fully investigated.
- The cause and significance of the unusual HTTP method value remained undetermined.
- The redirect destinations and the broader purpose of the traffic had not been independently established.

The absence of a particular log file should not, by itself, be treated as conclusive proof that the corresponding activity was impossible or absent from the capture.

## Key Learning Outcomes

This lab demonstrates how Zeek converts packet captures into structured network evidence. It also reinforces the importance of distinguishing observed behavior from an analyst's interpretation.

A useful investigation records what the evidence establishes, identifies unresolved questions, and recommends specific follow-up steps instead of making unsupported claims.

## Disclaimer

This repository documents a learning lab using a supplied packet capture. Findings are limited to the evidence reviewed and should not be interpreted as confirmation of malicious activity or a real-world security incident.
