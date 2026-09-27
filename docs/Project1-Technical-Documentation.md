# Project 1 – AWS VPC + EC2 Web Server

## 1. Project Objective

The objective of this project was to build a basic web server infrastructure
on AWS using a custom VPC and deploy an Apache web server on an EC2 instance.

The project demonstrates fundamental AWS networking, EC2 deployment,
Linux administration, security configuration, and troubleshooting.

---

## 2. AWS Region

- Region: Europe (Stockholm)
- Region Code: `eu-north-1`
- Availability Zone: `eu-north-1a`

---

## 3. VPC Configuration

A custom VPC was created to provide an isolated network environment for
the project.

| Component | Configuration |
|---|---|
| VPC Name | `cloud-project1-vpc` |
| CIDR Block | `10.0.0.0/16` |

The VPC provides the overall IP address range for the project.

---

## 4. Subnet Configuration

Two subnets were created inside the VPC.

### Public Subnet

| Property | Value |
|---|---|
| Name | `project1-public-subnet` |
| CIDR | `10.0.1.0/24` |
| Availability Zone | `eu-north-1a` |
| Purpose | Host the public-facing EC2 web server |

### Private Subnet

| Property | Value |
|---|---|
| Name | `project1-private-subnet` |
| CIDR | `10.0.2.0/24` |
| Availability Zone | `eu-north-1a` |
| Purpose | Reserved for future private resources |

The EC2 web server was deployed in the public subnet.

---

## 5. Internet Gateway

An Internet Gateway was created and attached to the VPC.

| Property | Value |
|---|---|
| Name | `project1-igw` |
| Purpose | Provide internet connectivity to resources in the public subnet |

The Internet Gateway allows traffic to flow between the VPC and the
internet when the appropriate route exists.

---

## 6. Route Table

A public route table was created and associated with the public subnet.

| Destination | Target |
|---|---|
| `10.0.0.0/16` | Local |
| `0.0.0.0/0` | Internet Gateway |

The `0.0.0.0/0` route allows internet-bound traffic from the public subnet
to reach the Internet Gateway.

---

## 7. Security Group

A Security Group was created for the EC2 web server.

| Property | Value |
|---|---|
| Name | `project1-web-sg` |

### Inbound Rules

| Protocol | Port | Source | Purpose |
|---|---:|---|---|
| TCP | 80 | `0.0.0.0/0` | Allow HTTP web traffic |
| TCP | 22 | Administrator's public IP `/32` | Allow SSH administration |

### Outbound Rules

All outbound traffic was allowed.



---

## 8. Network ACL

The public subnet was associated with a Network ACL.

### Inbound

- Rule 100: Allow all traffic from `0.0.0.0/0`
- Default rule: Deny

### Outbound

- Rule 100: Allow all traffic to `0.0.0.0/0`
- Default rule: Deny

The NACL configuration was checked during SSH connectivity troubleshooting.

---

## 9. EC2 Instance

An EC2 instance was launched inside the public subnet.

| Property | Value |
|---|---|
| Name | `project1-web-server` |
| AMI | Amazon Linux 2023 |
| Instance Type | `t3.micro` |
| Availability Zone | `eu-north-1a` |
| Subnet | `project1-public-subnet` |
| Private IP | `10.0.1.206` |
| Web Server | Apache HTTP Server |

The instance was successfully launched and passed its status checks.

---

## 10. SSH Configuration

The EC2 instance was accessed using SSH from Windows PowerShell.

The connection used an RSA private key associated with the EC2 key pair.

Example command:

```bash
ssh -i ".\project1-key.pem" ec2-user@<EC2-Public-IP>