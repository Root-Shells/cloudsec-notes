---
title: AWS Networking
description: Reference notes for AWS network design, VPCs, routing, segmentation, observability, and secure connectivity patterns.
tags:
  - aws
  - networking
  - cloud-security
---

AWS networking is the foundation for secure workload placement, private connectivity, segmentation, observability, and blast-radius control.

## Topics

- [[aws-networking/virtual-private-clouds/index|Virtual Private Clouds (VPCs)]]

## Reference Mindset

- Know what is reachable over the network.
- Know what is authorized by identity policy.
- Treat network reachability and IAM authorization as separate control planes.
- Prefer repeatable network patterns over one-off subnet and route table decisions.
- Design for investigation: routing, logging, traffic metadata, and packet inspection all matter.
