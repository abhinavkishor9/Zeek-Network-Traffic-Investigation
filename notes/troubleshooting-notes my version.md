
# Troubleshooting Notes

## 1. Environment
- Operating system: Ubuntu 24.04 on WSL
- Tool: Zeek 8.0.10
- Lab directory: `~/zeek-labs/lab01`
- Capture file: `quickstart.pcap`

## 2. Issue: Verifying the Zeek Installation

### Observation
Zeek was available at `/opt/zeek/bin/zeek`.

### Command
```bash
/opt/zeek/bin/zeek --version
```

### Result
```text
/opt/zeek/bin/zeek version 8.0.10
```

### Resolution
Used the verified absolute path to execute Zeek and process the capture.

## 3. Issue: Accessing the PCAP from WSL

### Observation
The capture was stored on the Windows D: drive.

### Command
```bash
cp "/mnt/d/important docs/Cybersecurity labs and resources/quickstart.pcap" ~/zeek-labs/lab01/
```

### Verification
```bash
cd ~/zeek-labs/lab01
ls -lh quickstart.pcap
file quickstart.pcap
```

### Result
The file was present in the lab directory and identified as a PCAP 2.4 Ethernet capture.

### Resolution
Copied the capture into the WSL lab directory before running Zeek.

## 4. Issue: DNS Log Not Generated

### Observation
Attempting to read `dns.log` returned:

```text
head: cannot open 'dns.log' for reading: No such file or directory
```

### Investigation
Checked the generated log files:

```bash
ls -lh *.log
```

The output contained `conn.log`, `files.log`, `http.log`, `packet_filter.log`, and `weird.log`. No `dns.log` was present.

### Assessment
The initial Zeek run did not produce a DNS log. This does not establish the reason why the log was absent, and DNS activity should not be inferred without supporting evidence.

### Follow-up
Review the capture and available protocol evidence before deciding whether further DNS-specific investigation is necessary.

## 5. Issue: TLS Log Not Generated

### Observation
Attempting to read `ssl.log` returned:

```text
head: cannot open 'ssl.log' for reading: No such file or directory
```

### Investigation
The inspected HTTP records used destination port 80 and were identified as HTTP transactions.

### Assessment
No `ssl.log` was generated during the initial run. The inspected records do not establish TLS activity. The absence of this log alone is not a complete analysis of every packet in the capture.

### Follow-up
Inspect the complete connection records and capture if determining whether TLS traffic exists becomes necessary.

## 6. Observation: Unusual HTTP Method

### Evidence
The second inspected HTTP record contains:

```text
method: WEIRD
host: zeek.org
uri: /
status_code: 301
status_msg: Moved Permanently
```

### Assessment
The method value differs from the `GET` method in the first HTTP record. This is an unusual observation that needs context, but it is not sufficient to confirm malicious behavior.

The `WEIRD` value in the HTTP method field is distinct from the contents of Zeek's separate `weird.log`.

### Follow-up
Review the relevant `weird.log` records and, if necessary, inspect the corresponding packets.

## 7. Issue: Generated Logs Not Yet Fully Reviewed

### Observation
The initial log directory contained:

- `conn.log`
- `files.log`
- `http.log`
- `packet_filter.log`
- `weird.log`

Only two connection records and two HTTP records had been examined in detail.

### Assessment
The existence of the remaining files does not establish what their contents mean or whether they contain relevant anomalies.

### Follow-up
Review the full contents of the remaining logs and update the investigation report with verified findings.

