# AWS VPC Peering Project

## Overview
This project demonstrates private communication between two Amazon VPCs using VPC Peering, configured through the AWS Management Console.

## Architecture

- **Region:** Asia Pacific (Mumbai) — `ap-south-1`
- **VPC-A CIDR:** `10.0.0.0/16`
- **VPC-B CIDR:** `10.1.0.0/16`
- **Public subnet:** `10.0.1.0/24`
- **Private subnet:** `10.1.1.0/24`
- **Peering connection:** Active

## EC2 Instances

| Instance | VPC | Private IP |
|---|---|---|
| EC2-Public-A | VPC-A | `10.0.1.239` |
| EC2-Private-B | VPC-B | `10.1.1.82` |

## Route Tables

- VPC-A routes `10.1.0.0/16` through the VPC peering connection.
- VPC-B routes `10.0.0.0/16` through the VPC peering connection.

## Validation

Command executed from EC2-Public-A:

```bash
ping -c 4 10.1.1.82
```

**Result:** 4 packets transmitted, 4 received, 0% packet loss. Average round-trip time: 0.738 ms.

This confirms successful ICMP connectivity between the two VPCs at the time of testing.

## Security

- EC2-Public-A is reachable through its public IP subject to its security-group rules.
- EC2-Private-B has no public IPv4 address.
- ICMP access is restricted by security-group rules.
- AWS credentials and private key files must never be committed to the repository.

## Evidence

See the `screenshots/` directory for configuration and connectivity evidence.

## Cleanup

After collecting the evidence, stop or terminate lab resources that are no longer needed. Terminate temporary EC2 instances and remove unnecessary resources to avoid further charges. Preserve unrelated resources such as existing Jenkins instances.
