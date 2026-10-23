# Network Traffic Investigation Using Wireshark

## Investigation Overview

This lab investigated network activity captured from a Windows endpoint
using Wireshark.

The investigation focused on identifying queried domains, contacted
remote endpoints, connection sequences, and traffic patterns. Packet
evidence was used to determine whether the observed activity matched
the actions performed during the capture and whether any activity
required further investigation.

## Investigation Scenario

A short packet capture was collected from a Windows endpoint during
controlled browsing and network activity.

The analyst's task was to reconstruct the activity from the capture,
identify relevant connections, and assess whether the observed traffic
could be explained by the recorded user actions.

This was a controlled lab investigation, not a confirmed security incident.

## Investigation Questions

1. Which endpoint generated the traffic?
2. Which domains were queried?
3. Which remote IP addresses and ports were contacted?
4. Could DNS responses be correlated with subsequent connections?
5. Did the selected TCP connections establish successfully?
6. What could be determined about encrypted web connections?
7. Which conversation transferred the most data?
8. Did the observed activity match the recorded user actions?
9. What remained unexplained?

## Environmenet

| Component | Details |
|---|---|
| Operating system | Windows 11 |
| Platform | Physical PC |
| Wireshark version | Version 4.6.9 |
| Capture interface | Wi-Fi |
| Network mode | Physical host network (Wi-Fi) |
| Endpoint IP address | `192.168.1.17` |
| Evidence file | `network-investigation.pcapng` |
| Investigation date | `2026-10-23` |

## Scope and Authorization

The investigation was limited to traffic captured from my own endpoint
or an explicitly authorized lab environment.

No unauthorized systems were scanned or accessed.

The capture was reviewed for sensitive information before any evidence
was published.

## Evidence Collection

### Collection procedure

1. Opened Wireshark.
2. Selected the active network interface.
3. Started the packet capture.
4. Recorded the actions performed and their approximate times.
5. Generated controlled network activity.
6. Stopped and saved the capture.
7. Preserved the original capture and used a copy for analysis.

### Controlled activity

The following commands were used during the capture:

```powershell
nslookup example.com
ping example.com -n 4
curl.exe --http1.1 -I [https://example.com](https://example.com)
```

Actual outcomes were recorded, including unsuccessful commands.

### Activity log

| Time | Action performed | Result |
|---|---|---|
| 14:42:18 GMT (23 Oct 2026) | Queried `example.com` using `nslookup` | DNS resolution succeeded. Returned IPv4 addresses `104.20.23.154` and `172.66.147.243`, along with IPv6 addresses. |
| 14:42:18 GMT (approx.) | Sent four ICMP echo requests using `ping example.com -n 4` | Received 4 replies out of 4; 0% packet loss. Round-trip times ranged from 14 ms to 21 ms. |
| 14:42:18 GMT (approx.) | Requested HTTPS response headers using `curl.exe --http1.1 -I https://example.com` | Received `HTTP/1.1 200 OK`. The response included `Content-Type: text/html; charset=utf-8` and `Server: cloudflare`. |

### Evidence inventory

| Evidence ID | Item | Description |
|---|---|---|
| E01 | `network-investigation.pcapng` | Original Wireshark packet capture containing the network traffic collected during the lab. |
| E02 | Screenshots | Screenshots documenting the capture overview, endpoint scope, DNS evidence, connection correlation, TCP handshake, TLS handshake information, and conversation statistics, where observed. |
| E03 | Activity log | Record of the commands executed and their observed results during controlled network testing. |


## Investigation 1 — Identify the Endpoint and Traffic Scope



### Analysis

The local endpoint IP address was identified as `192.168.1.17` and compared with the source and destination addresses shown in Wireshark.

The capture contained 1,002 packets in total. Filtering for traffic involving the endpoint displayed 427 packets (42.6% of the capture), with zero dropped packets reported.

The visible traffic included TCP and TLSv1.3 communications with remote endpoints, including HTTPS traffic over port 443. The capture was reviewed to identify DNS, ICMP, TCP, and encrypted TLS activity.

### Display filter

```text
ip.addr == 192.168.1.17
```

### Findings

| Field | Finding |
|---|---|
| Endpoint IP | `192.168.1.17` |
| Capture duration |[2026-10-23 20:15:54-2026-10-23 20:16:54]|
| Total packets | 1,002 |
| Protocols observed | TCP, TLSv1.3 |


### Assessment

The capture included traffic associated with the endpoint `192.168.1.17`, including TCP connections and TLSv1.3 application data exchanged with remote IP addresses. The endpoint filter displayed 427 of the 1,002 captured packets, and Wireshark reported zero dropped packets.

The analysis was limited to the available capture and its recorded time window. The presence of encrypted traffic allowed connection metadata to be examined, but did not by itself reveal the contents of the encrypted application data.


## Investigation 2 — Identify Queried Domains


### Display filter

```text
dns
```

### Analysis

1. Identified a DNS query.
2. Recorded the requested domain and query type.
3. Located the corresponding response.
4. Compared the transaction ID and request/response information.
5. Recorded the returned address or error response.

### Findings

| Query packet | Response packet | Domain | Query type | Answer |
|---:|---:|---|---|---|
| 668 | 669 | `arc.msn.com` | AAAA | CNAME records visible; final IPv6 answer not yet verified |
| 959 | 960 | `browser-intake-datadoghq.com` | A | `3.233.158.26`, `3.233.158.25`, `3.233.158.24` |

### Assessment

The capture contains DNS queries and corresponding responses. An AAAA query was observed for `arc.msn.com`, and an A query for `browser-intake-datadoghq.com` returned three IPv4 addresses. This confirms DNS resolution activity in the capture. The observed activity is consistent with DNS requests made during normal endpoint activity, but a complete comparison with the activity log is needed for confirmation. Neither domain is classified as malicious based on this evidence alone.


## Investigation 3 — Correlate DNS With a Connection



### Analysis

1. Selected an address from the DNS response.
2. Filtered for traffic involving that address.
3. Compared packet timestamps.
4. Located a subsequent connection from the endpoint.
5. Recorded the destination port and TCP stream index.

### Example display filter

```text
ip.addr == 3.233.158.26
```

This example address was replaced with an address actually returned
in the capture.

### Findings

### Findings

| Field | Finding |
|---|---|
| Queried domain | `browser-intake-datadoghq.com` |
| Returned IP address | `3.233.158.26` |
| DNS response packet | `960` |
| Connection start packet | `975` |
| Destination port | `443` |
| TCP stream index | `30` |

### Assessment

The DNS response in packet 960 returned the IP address `3.233.158.26` for `browser-intake-datadoghq.com`. A subsequent TCP SYN packet (packet 975) showed the endpoint connecting to the same IP address on destination port 443. The matching destination address and packet sequence support a relationship between the DNS response and the connection attempt. However, the TCP conversation was marked `SYN_SENT` and incomplete, so a successful connection could not be confirmed from the captured evidence.

## Investigation 4 — Examine Connection Establishment



### Display filter

```text
tcp.stream eq 0
```

The stream index was replaced with the value from the selected connection.

### Analysis

The selected stream was reviewed for:

- Client SYN.
- Server SYN-ACK.
- Client ACK.
- Subsequent data exchange.
- Connection termination or reset, if captured.

### Findings

### Findings

| Stage | Packet number | Source → Destination | Observation |
|---|---:|---|---|
| SYN | 975 | `192.168.1.17 → 3.233.158.26:443` | Initial connection request observed |
| SYN-ACK | Not observed in available evidence | — | Server response not confirmed |
| ACK | Not observed in available evidence | — | Handshake completion not confirmed |
| Later activity | Not confirmed | — | No subsequent data exchange established |

### Assessment

The selected TCP stream (index 30) showed an initial SYN packet (packet 975) from the endpoint to `3.233.158.26` on destination port 443. Wireshark marked the conversation as `SYN_SENT` and incomplete. The available evidence does not confirm that the server returned a SYN-ACK or that the TCP three-way handshake completed. Therefore, a successful connection could not be established from the captured evidence.

## Investigation 5 — Examine Encrypted Web Activity

### Display filter

```text
tls
```

### Analysis

The selected connection was reviewed for available handshake information,
including the Client Hello, Server Hello, and server name where visible.

### Findings

| Field | Finding |
|---|---|
| Client endpoint | IPv6 client, source port `61333` |
| Server endpoint | `browser-intake-datadoghq.com`, port `443` |
| Client Hello packet | `974` |
| Server Hello packet | `986` |
| Server name | `browser-intake-datadoghq.com` |
| Negotiated TLS version | TLS 1.3 (as labelled by Wireshark) |
| Application content readable | No — TLS Application Data is encrypted |

### Assessment

The capture contains a TLS handshake for browser-intake-datadoghq.com in TCP stream 28. Packet 974 shows the Client Hello and exposes the server name, while packet 986 shows a Server Hello. Wireshark labels the traffic TLS 1.3, and subsequent packets contain encrypted Application Data. The capture therefore provides evidence of encrypted web communication, but the application content cannot be interpreted from these packets alone.



## Investigation 6 — Assess Conversation Volume


### Analysis

1. Opened Statistics → Conversations.
2. Selected the relevant IP or transport tab.
3. Checked whether the statistics were limited to the display filter.
4. Sorted by total bytes.
5. Selected the largest relevant conversation.
6. Compared its timing and endpoints with the activity log.

### Findings

| Endpoint A | Endpoint B | Packets | Bytes | Duration |
|---|---|---:|---:|---|
| `192.168.1.17` | `104.18.32.47` | 174 | 118 kB | 39.289 seconds |

### Assessment

The largest TCP conversation in the capture was between the endpoint `192.168.1.17` and `104.18.32.47`, transferring approximately 118 kB across 174 packets over about 39.29 seconds. The traffic used destination port 443, consistent with HTTPS communication. The endpoint and port are consistent with web activity, but the available statistics alone do not identify the application or confirm exactly which lab command generated the traffic. A large byte count alone is not evidence of malicious activity.

## Investigation Timeline

| Time and timezone | Packet number | Event | Interpretation |
|---|---:|---|---|
| `59.796 seconds` (capture-relative) | 959 | DNS query for `browser-intake-datadoghq.com` | Endpoint requested an A record |
| `59.796 seconds` (capture-relative) | 960 | DNS response containing `3.233.158.26` | DNS response also listed `3.233.158.25` and `3.233.158.24` |
| `60.358785800` (capture-relative) | 975 | TCP SYN to `3.233.158.26:443` | Endpoint initiated a TCP connection attempt |
| Not confirmed | — | TCP handshake completed | The reviewed stream was marked `SYN_SENT` and incomplete |
| `60.073339900` (capture-relative) | 974 | TLS Client Hello for `browser-intake-datadoghq.com` | TLS handshake activity was observed in a different stream (stream 28) |
| `60.368673600` (capture-relative) | 986 | TLS Server Hello | Server Hello observed in stream 28 |
| Not confirmed | — | Connection closed or reset | Closure or reset was not established by the reviewed evidence |

*The times above are capture-relative seconds shown in Wireshark, not wall-clock times. The DNS rows show approximately 59.796 seconds in the screenshot; inspect the packet timestamps if you need more precise values.*

## Findings Summary

| Investigation question | Answer | Supporting evidence |
|---|---|---|
| Which endpoint was investigated? | `192.168.1.17` | Endpoint filter and TCP packet 975 |
| Which domains were queried? | Examples include `arc.msn.com` and `browser-intake-datadoghq.com` | DNS packets 668–669 and 959–960 |
| Which remote endpoints were contacted? | `3.233.158.26:443` was the destination of a TCP connection attempt; other TCP conversations are listed in Conversations statistics | Packet 975; Conversations statistics |
| Was DNS correlated with a connection? | Yes, the matching IP supports a relationship between DNS resolution and a connection attempt | DNS response 960 and TCP SYN 975 |
| Did the TCP handshake complete? | Not confirmed for stream 30 | Packet 975; Wireshark reported `SYN_SENT` and an incomplete conversation |
| What did TLS metadata reveal? | TLS 1.3-labelled traffic, Client Hello with SNI `browser-intake-datadoghq.com`, and a Server Hello | Packets 974 and 986, TCP stream 28 |
| What was the largest conversation? | `192.168.1.17` ↔ `104.18.32.47`, 174 packets, 118 kB, about 39.29 seconds | Conversations statistics; stream ID shown as `0` |
| Was any activity unexplained? | No specific malicious activity was established, but the available evidence does not explain every connection's originating process | Packet analysis and limitations below |

## Conclusion

The investigation identified DNS queries including `arc.msn.com` and `browser-intake-datadoghq.com`, TCP traffic to remote endpoints on port 443, and TLS 1.3-labelled handshake activity. The DNS response for `browser-intake-datadoghq.com` returned `3.233.158.26`, and a subsequent TCP SYN from `192.168.1.17` targeted that address.

The traffic partially matched the recorded lab activity because DNS and HTTPS-related traffic were observed, but the capture alone did not attribute every connection to a specific command or application. The selected TCP stream 30 remained incomplete in the reviewed evidence, while a separate TLS exchange was visible in stream 28.

No malicious activity was established from the evidence reviewed. This conclusion applies only to the captured traffic and does not establish that the endpoint was free from compromise.

## Limitations

- The capture covered a limited time window.
- Encrypted application content was not readable from the available packets.
- Packet evidence alone did not identify every originating process.
- The selected TCP stream 30 was marked incomplete, so a successful handshake was not confirmed for that stream.
- Some packet timestamps are capture-relative, and the capture's wall-clock timezone was not verified.
- Any unreviewed traffic remains outside this investigation's conclusions.

## Recommended Next Steps

- Correlate unexplained connections with endpoint logs if process attribution is needed.
- Review additional capture time if an event needs clarification.
- Confirm whether remote endpoints were associated with expected software or browser activity.
- Retain the packet capture and screenshots as supporting evidence for the investigation.

## What I Learned

- How to frame a network investigation around specific questions.
- How to identify relevant traffic in a packet capture.
- How to correlate DNS responses with later connections.
- How to reconstruct a connection timeline.
- How to distinguish observations from interpretations.
- How to document conclusions and limitations.

## References

- [Wireshark User's Guide](https://www.wireshark.org/docs/wsug_html/)
- [Wireshark Display Filters](https://wiki.wireshark.org/DisplayFilters)
- [Wireshark TLS](https://wiki.wireshark.org/TLS)

