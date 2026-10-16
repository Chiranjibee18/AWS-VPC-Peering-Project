# AWS VPC Peering Project

**Connecting Two AWS VPCs Through Private Networking**

## 1. Project Overview

In this project, I created two separate Virtual Private Clouds (VPCs) in Amazon Web Services (AWS) and connected them using VPC Peering.

The main purpose of this project was to understand how two different VPCs can communicate with each other using private IP addresses without sending their communication traffic through the public internet.

I completed the project using the AWS Management Console. I created VPCs and subnets, configured an Internet Gateway, established a VPC Peering connection, updated route tables, configured security groups, and launched EC2 instances to test connectivity.

Finally, I used the `ping` command to verify that communication between the two VPCs was working successfully.

## 2. Project Objectives

The main objectives of this project were:

- To understand the basics of AWS VPC and subnetting.
- To create and configure two separate VPCs.
- To understand the purpose of public and private subnets.
- To configure an Internet Gateway for internet connectivity.
- To establish a VPC Peering connection.
- To configure route tables for communication between VPCs.
- To configure security groups to control network traffic.
- To launch and configure EC2 instances.
- To test private communication using the `ping` command.
- To document the implementation with screenshots and validation results.

## 3. Technologies and AWS Services Used

| Service or Technology | Purpose |
|---|---|
| Amazon VPC | Creates isolated virtual networks in AWS. |
| Amazon EC2 | Provides virtual machines for testing connectivity. |
| VPC Peering | Connects the two VPCs privately. |
| Internet Gateway | Enables internet connectivity for the public subnet when routing and security rules permit it. |
| Route Tables | Decide where network traffic should be sent. |
| Security Groups | Control allowed inbound and outbound traffic. |
| Ubuntu Linux | Operating system used on the EC2 instances. |
| SSH | Provides secure remote access to the public EC2 instance. |
| ICMP Ping | Tests network connectivity between instances. |

## 4. Project Architecture

The project contains two VPCs:

### VPC-A

VPC-A is the first virtual network. It contains a public subnet with an EC2 instance used to initiate connectivity tests.

- VPC CIDR: `10.0.0.0/16`
- Public subnet CIDR: `10.0.1.0/24`
- EC2 private IP: `10.0.1.239`
- EC2 public IP at the time of testing: `13.233.38.136`

The public EC2 instance allows remote SSH access according to its security-group rules. An Internet Gateway is attached to VPC-A to support internet connectivity.

### VPC-B

VPC-B is the second virtual network. It contains a private subnet with an EC2 instance that has no public IPv4 address.

- VPC CIDR: `10.1.0.0/16`
- Private subnet CIDR: `10.1.1.0/24`
- EC2 private IP: `10.1.1.82`
- Public IP: None

The private EC2 instance is accessed through private network communication rather than direct public internet access.

### VPC Peering Connection

I created a VPC Peering connection between VPC-A and VPC-B. Once the connection became active, I configured the route tables to direct traffic between the two VPC CIDR ranges.

This allows the EC2 instances to communicate using their private IP addresses.

## 5. Implementation Steps

### Step 1: Create VPC-A

I created the first VPC with the CIDR block `10.0.0.0/16`. This network contains the public subnet used for the first EC2 instance.

### Step 2: Create VPC-B

I created a second VPC with the CIDR block `10.1.0.0/16`. I used a different CIDR range to prevent overlap between the two VPCs.

### Step 3: Create the Subnets

I created the public subnet `10.0.1.0/24` in VPC-A and the private subnet `10.1.1.0/24` in VPC-B.

A subnet divides a VPC into smaller networks. A public subnet can support internet connectivity when correctly configured, while a private subnet is designed to keep resources from being directly accessible through a public IP address.

### Step 4: Configure the Internet Gateway

I attached an Internet Gateway to VPC-A and configured the public subnet's route table for internet-bound traffic.

The Internet Gateway provides the network path to the internet. A public IP address, suitable routes, and appropriate security rules are also required for an EC2 instance to be reachable from the internet.

### Step 5: Create the VPC Peering Connection

I created a peering connection between VPC-A and VPC-B. I verified that the peering connection status became **Active**.

A peering connection creates a private network link between the VPCs, but it does not automatically configure their route tables.

### Step 6: Configure the Route Tables

I added the required routes to both VPC route tables.

**In VPC-A:**

- Destination: `10.1.0.0/16`
- Target: VPC Peering connection

**In VPC-B:**

- Destination: `10.0.0.0/16`
- Target: The same VPC Peering connection

These routes tell each VPC how to send traffic to the other VPC. Both sides need the correct routes so that requests and replies can travel successfully.

### Step 7: Launch the EC2 Instances

I launched two Ubuntu EC2 instances.

The first instance, EC2-Public-A, was launched in the public subnet of VPC-A. It has a private IP address of `10.0.1.239` and a public IPv4 address for remote access.

The second instance, EC2-Private-B, was launched in the private subnet of VPC-B. It has a private IP address of `10.1.1.82` and no public IPv4 address.

### Step 8: Configure Security Groups

I configured `SG-Public-Bastion` for EC2-Public-A and `SG-Private-Test` for EC2-Private-B.

The public instance's security group allows SSH access from an authorized source. The private instance's security group allows inbound ICMP traffic from VPC-A's CIDR range (`10.0.0.0/16`).

These rules allow the intended connectivity test while controlling which traffic is permitted.

## 6. Connectivity Testing

After completing the network configuration, I connected to EC2-Public-A through SSH.

From the public instance, I executed the following command to test connectivity to the private EC2 instance:

```bash
ping -c 4 10.1.1.82
```

The command sent four ICMP packets to the private IP address of EC2-Private-B.

### Test Results

- Packets transmitted: 4
- Packets received: 4
- Packet loss: 0%
- Average round-trip time: Approximately 0.738 ms

All four packets received replies. This confirmed that private ICMP communication between the two VPCs was working at the time of testing.

The successful test demonstrated that the VPC Peering connection, route tables, and relevant security-group rules were configured correctly for this traffic.

## 7. Security Considerations

During this project, I followed these basic security practices:

- Kept the private EC2 instance without a public IPv4 address.
- Used security groups to control network access.
- Restricted SSH access to an authorized source rather than allowing it from everywhere.
- Used private IP addresses for communication between the VPCs.
- Kept the EC2 private key file out of the GitHub repository.

VPC Peering provides a private network path, but it does not automatically encrypt all traffic. Security groups and route tables must still be configured correctly.

## 8. Screenshots and Evidence

The repository contains screenshots documenting the project configuration and testing.

The evidence should include:

1. VPC overview showing VPC-A and VPC-B.
2. Subnet configuration.
3. Active VPC Peering connection.
4. Route table configuration for VPC-A.
5. Route table configuration for VPC-B.
6. EC2 instance configuration and IP addresses.
7. Successful ping output showing zero packet loss.
8. Architecture diagram illustrating the network design.

These screenshots provide evidence of the implementation and its successful connectivity test.

## 9. Challenges Faced and Solutions

During the project, I encountered some EC2 launch issues, including an AWS restriction involving T2 Unlimited mode and temporary instance capacity limitations in an Availability Zone.

I addressed these issues by selecting a suitable available instance type and correcting the VPC and subnet selections during instance creation.

I also learned the importance of verifying an EC2 instance's VPC, subnet, IP addresses, and security group instead of relying only on its name.

These troubleshooting steps helped me understand AWS networking more practically.

## 10. What I Learned

By completing this project, I learned:

- How to create and configure AWS VPCs and subnets.
- How public and private subnets differ.
- How an Internet Gateway enables internet connectivity.
- How VPC Peering connects separate VPCs.
- Why both route tables need appropriate routes.
- How security groups control network traffic.
- How to launch and configure EC2 instances.
- How to use SSH and ping to test connectivity.
- How to troubleshoot incorrect network configurations.
- How to document a cloud project using GitHub.

## 11. Conclusion

This project gave me practical experience with AWS networking and VPC Peering. I successfully connected two separate VPCs and verified communication between their EC2 instances using private IP addresses.

The successful ping test, with four packets received and zero packet loss, demonstrated that the configured network path was working.

This project strengthened my understanding of AWS networking fundamentals and provided a foundation for learning more advanced cloud and DevOps concepts.

## 12. Cleanup

After collecting the required screenshots and project evidence, I will stop or terminate the EC2 instances and remove unnecessary lab resources to avoid additional AWS charges. Existing resources unrelated to this project will be preserved.

---

**Project status:** Private connectivity between VPC-A and VPC-B was successfully tested using ICMP ping.
