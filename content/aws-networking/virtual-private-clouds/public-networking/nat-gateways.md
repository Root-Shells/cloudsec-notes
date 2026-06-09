---
title: NAT Gateways
description: Managed NAT gateway architecture, private subnet outbound access, translation flow, route table design, resiliency, and security considerations.
tags:
  - aws
  - vpc
  - nat-gateway
  - private-subnet
  - egress
---

A NAT gateway is the recommended AWS-managed service for giving private subnet resources outbound access without allowing unsolicited inbound connections from the destination network.

For typical public internet egress, a public NAT gateway sits in a public subnet, uses an Elastic IP address, and sends traffic onward through the VPC internet gateway.

## Public NAT Gateway Pattern

```mermaid
flowchart TD
    Internet["Public internet / AWS public endpoints"]
    IGW["Internet gateway"]

    subgraph VPC["VPC"]
        PublicSubnet["Public subnet"]
        NAT["Public NAT gateway<br/>private IP + Elastic IP"]
        PrivateSubnet["Private app subnet"]
        App1["Instance 10.16.32.10"]
        App2["Instance 10.16.32.20"]
        PrivateRT["Private route table<br/>0.0.0.0/0 -> NAT"]
        PublicRT["Public route table<br/>0.0.0.0/0 -> IGW"]
    end

    App1 --> PrivateRT --> NAT
    App2 --> PrivateRT --> NAT
    NAT --> PublicRT --> IGW --> Internet
```

Important points:

- The NAT gateway is placed in a public subnet.
- Private subnet route tables point `0.0.0.0/0` at the NAT gateway.
- The NAT gateway subnet route table points `0.0.0.0/0` at the internet gateway.
- The NAT gateway needs an Elastic IP address when it is a public NAT gateway.
- External systems see the NAT gateway's public IPv4 address, not the private instance address.

## Translation Flow

```mermaid
sequenceDiagram
    participant App as Private instance 10.16.32.10
    participant NAT as NAT Gateway
    participant IGW as Internet Gateway
    participant Dest as Public service 1.3.3.7

    App->>NAT: Source 10.16.32.10, destination 1.3.3.7
    NAT->>NAT: Record translation state
    NAT->>IGW: Source NAT gateway private IP
    IGW->>Dest: Source NAT gateway Elastic IP
    Dest-->>IGW: Response to Elastic IP
    IGW-->>NAT: Translate back to NAT gateway private IP
    NAT-->>App: Translate back to 10.16.32.10
```

The NAT gateway tracks connection state so it can map response traffic back to the correct private source.

## Resiliency Pattern

Use one NAT gateway per Availability Zone for production resiliency. Route private subnets to the NAT gateway in the same AZ when practical.

```mermaid
flowchart LR
    AppA["Private subnet A"] --> NATA["NAT gateway A"]
    AppB["Private subnet B"] --> NATB["NAT gateway B"]
    AppC["Private subnet C"] --> NATC["NAT gateway C"]
    NATA --> IGW["Internet gateway"]
    NATB --> IGW
    NATC --> IGW
```

If several AZs share one NAT gateway and that NAT gateway's AZ has a problem, private subnets in other AZs can lose internet egress.

## IPv6 Nuance

Native IPv6 does not need IPv4-style NAT. For outbound-only native IPv6 access, use an egress-only internet gateway.

Modern AWS NAT gateways can also support IPv6-to-IPv4 egress with DNS64/NAT64 patterns. Treat that as a specific translation design, not the default answer for native IPv6 internet access.

## Security Considerations

- NAT hides private source addresses from the public internet, but it does not inspect application intent.
- Private subnets with NAT egress can still exfiltrate data.
- Restrict egress with security groups, NACLs, network firewalling, proxy controls, or endpoint-first designs where needed.
- Prefer VPC endpoints for supported AWS services to avoid unnecessary public-path egress.
- Monitor NAT gateway bytes, connection errors, and destination patterns.

## AWS References

- [NAT gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)
- [NAT gateway basics](https://docs.aws.amazon.com/vpc/latest/userguide/nat-gateway-basics.html)
- [VPC with private subnets and NAT](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-example-private-subnets-nat.html)
