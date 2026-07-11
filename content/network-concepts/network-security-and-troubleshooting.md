---
title: Network Security and Troubleshooting
description: Condensed reference for firewalls, filtering, port scanning, input trust, observability, and practical network troubleshooting.
tags:
  - networking
  - security
  - troubleshooting
  - detection
---

Network security starts with reachability, but it does not end there. A host can be reachable and still reject a request at TLS, authentication, authorization, or application validation layers.

## Firewalls

Firewalls apply policy to traffic. They may inspect IP addresses, ports, protocols, connection state, users, application signatures, TLS metadata, or payloads depending on the device and deployment model.

```mermaid
flowchart TD
    Packet["Packet arrives"]
    Direction["Ingress or egress policy"]
    Match{"Rule match?"}
    State{"Existing allowed state?"}
    Allow["Allow and log"]
    Deny["Deny or drop and log"]

    Packet --> Direction --> State
    State -->|Yes| Allow
    State -->|No| Match
    Match -->|Allow rule| Allow
    Match -->|Deny or no match| Deny
```

Stateful firewalls remember allowed conversations and permit return traffic automatically. Stateless firewalls evaluate every packet independently, so inbound and outbound rules must both be correct.

## Port Scanning

Port scanning tests what services appear reachable. It is useful for inventory, validation, and attacker reconnaissance.

Interpretation matters:

| Result        | Possible Meaning                                                |
| ------------- | --------------------------------------------------------------- |
| Open          | A service responded on that port                                |
| Closed        | Host reachable, no service listening                            |
| Filtered      | Firewall, ACL, routing, or packet loss prevented a clear answer |
| Open/filtered | Common with UDP when no response is received                    |

Scanning finds exposure, not full risk. Risk depends on service identity, version, authentication, authorization, data sensitivity, and exploitability.

## Input Trust

Any network-facing program must treat input as hostile unless proven otherwise. Protocol correctness is not the same as application safety.

Controls:

- Parse with strict bounds and expected formats.
- Authenticate callers before sensitive operations.
- Authorize per action, not just per connection.
- Avoid trusting client-provided source identity headers unless inserted by a trusted proxy.
- Log security-relevant failures without leaking secrets.
- Rate-limit expensive operations and unauthenticated endpoints.

## Practical Troubleshooting Flow

```mermaid
flowchart TD
    Start["Connection fails"]
    DNS{"Name resolves?"}
    Route{"Route exists both ways?"}
    Link{"Local link and gateway work?"}
    Filter{"Firewall allows path?"}
    Listen{"Service listening?"}
    TLS{"TLS succeeds?"}
    App{"Application accepts request?"}
    Done["Working path identified"]

    Start --> DNS
    DNS -->|No| FixDNS["Fix DNS or resolver path"]
    DNS -->|Yes| Route
    Route -->|No| FixRoute["Fix route table, gateway, tunnel, or return path"]
    Route -->|Yes| Link
    Link -->|No| FixLink["Fix interface, VLAN, ARP, gateway, or local network"]
    Link -->|Yes| Filter
    Filter -->|No| FixFilter["Fix firewall, security group, NACL, or proxy policy"]
    Filter -->|Yes| Listen
    Listen -->|No| FixListen["Start service or correct listener binding"]
    Listen -->|Yes| TLS
    TLS -->|No| FixTLS["Fix certificate, SNI, trust chain, or protocol version"]
    TLS -->|Yes| App
    App -->|No| FixApp["Fix auth, authorization, routing, or application logic"]
    App -->|Yes| Done
```

## Useful Evidence

| Evidence           | What It Answers                                             |
| ------------------ | ----------------------------------------------------------- |
| DNS query logs     | Which names were requested and resolved                     |
| Flow logs          | Who connected to whom, on which ports, accepted or rejected |
| Packet capture     | What packets actually appeared at a point in the path       |
| Firewall logs      | Which rule allowed or denied traffic                        |
| Load balancer logs | Client-facing request status and backend target behavior    |
| Application logs   | Auth decisions, request IDs, errors, business context       |

## Security Engineering Patterns

- Maintain an expected exposure inventory.
- Alert when new internet-facing listeners appear.
- Treat public DNS changes as production security changes.
- Monitor blocked traffic patterns for scanning and brute force attempts.
- Prefer least-reachability networks, then enforce identity and application authorization above them.
- Use packet path diagrams during design reviews and incident response.

## References

- [Beej's Guide to Network Concepts](https://beej.us/guide/bgnet0/html/split/index.html)
- [NIST SP 800-41 Rev. 1: Guidelines on Firewalls and Firewall Policy](https://csrc.nist.gov/publications/detail/sp/800-41/rev-1/final)
- [OWASP Input Validation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html)
