# Zeek-Network-Traffic-Investigation
## Overview
You are a SOC analyst investigating a packet capture collected from a network under investigation. You have no prior incident report, no confirmed malicious IP addresses, and no established indicators of compromise.
Your task is to process the capture with Zeek, examine the generated network logs, identify the observed communications, and document anything that warrants further investigation.
The objective is not to label traffic malicious immediately. It is to establish what happened based on the available network evidence.

This lab introduces network traffic investigation using Zeek, an open-source network security monitoring tool. The investigation uses an offline PCAP capture to examine connection metadata, HTTP transactions, and potentially unusual protocol behavior.

The objective is to understand how network evidence can be extracted, correlated, and documented without automatically treating unusual activity as malicious.

## Lab Objectives

- Understand how Zeek processes packet captures and converts network traffic into structured logs.
- Perform offline network traffic analysis using `quickstart.pcap` in Ubuntu 24.04 on WSL.
- Identify communicating hosts, source and destination ports, transport protocols, connection durations, and connection states from `conn.log`.
- Examine HTTP transaction metadata, including request methods, hostnames, URIs, user agents, and response status codes.
- Correlate connection records with HTTP transactions using Zeek connection identifiers.
- Investigate the unusual `WEIRD` HTTP method value and determine what additional evidence is required to explain it.
- Examine Zeek's generated logs to identify relevant protocol anomalies and file-related activity.
- Understand why DNS or TLS logs may not be generated during a particular analysis and avoid drawing conclusions from missing logs alone.
- Distinguish confirmed network observations from potential security concerns and unsupported assumptions.
- Document evidence limitations, investigation gaps, and appropriate follow-up actions in a structured network investigation report.

## Lab Environment

| Component | Details |
|---|---|
| Operating system | Ubuntu 24.04 on WSL |
| Network analysis tool | Zeek 8.0.10 |
| Analysis mode | Offline PCAP analysis |
| Capture file | `quickstart.pcap` |
| Capture format | PCAP 2.4, Ethernet |
| Analysis directory | `~/zeek-labs/lab01` |


## Lab Scenario 

A security analyst receives a small packet capture named `quickstart.pcap` for an initial network traffic investigation. The objective is to examine the recorded communications, understand the observed HTTP activity, and identify any behavior that requires further analysis using Zeek.

The investigation is performed offline in Ubuntu 24.04 on WSL using Zeek 8.0.10. After processing the capture, the analyst reviews the generated connection and HTTP logs to establish which hosts communicated, which protocols and ports were involved, and what application-level activity was recorded.

### Investigation Context

The initial analysis identifies two TCP connections originating from `192.168.1.8` and targeting `192.0.78.212:80` and `192.0.78.150:80`. Both connections are identified as HTTP, and the corresponding HTTP records reference `zeek.org` with a response status of `301 Moved Permanently`.

One request contains the method value `WEIRD` rather than the `GET` method observed in the other transaction. This difference raises a question about the request's format and context, but it does not independently establish malicious activity.

### Analyst Tasks

- Review `conn.log` to identify communicating hosts, destination ports, connection states, and durations.
- Examine `http.log` to understand the recorded requests, user-agent values, and HTTP responses.
- Investigate the unusual HTTP method value and determine whether additional evidence is needed to explain it.
- Review the other generated logs, including `weird.log` and `files.log`, for relevant supporting evidence.
- Document confirmed observations, unresolved questions, evidence limitations, and recommended follow-up actions.

### Expected Outcome

The analyst should produce an evidence-based investigation report that explains the observed network activity and identifies areas requiring further examination. Conclusions must remain limited to what the available logs and packet evidence support.

The investigation should not classify the traffic as malicious solely because an HTTP method appears unusual or because particular Zeek log files were not generated.

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

