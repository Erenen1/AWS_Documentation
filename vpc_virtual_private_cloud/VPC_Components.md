# VPC Components

## Subnets (Public and Private Subnets)
A subnet is a segment of the VPC's IP address range where you can place AWS resources. 
- **Public Subnets**: Connected to the Internet through an Internet Gateway. Resources like web servers are typically hosted in public subnets.
- **Private Subnets**: Isolated from the Internet. Resources like databases and backend services are typically hosted here.
- **Design Consideration**: Use Network ACLs and Security Groups to control traffic flow between subnets.

## Route Tables
Route Tables define the rules for directing network traffic within a VPC.
- **Main Route Table**: Automatically created for each VPC and applies to all subnets unless explicitly associated with a custom route table.
- **Custom Route Tables**: Created to define specific routing rules, such as routing traffic through a NAT Gateway.
- **Routing Targets**: Includes local VPC traffic, Internet Gateway, NAT Gateway, and peered VPCs.

## Internet Gateway
An Internet Gateway allows resources in public subnets to connect to the Internet.
- **Key Features**:
  - Scalable, redundant, and highly available.
  - Ensures two-way communication between VPC resources and the Internet.
- **Use Case**: Hosting public-facing applications.

## NAT Gateway/NAT Instance
These enable outbound Internet connectivity for resources in private subnets while keeping them unreachable from the Internet.
- **NAT Gateway**:
  - Fully managed by AWS.
  - Highly available and scales automatically.
- **NAT Instance**:
  - User-managed EC2 instance.
  - Requires manual setup and maintenance.
- **Use Case**: Securely allow private subnet resources to access software updates or external APIs.

## Elastic IP Addresses
Elastic IP (EIP) is a static, public IPv4 address designed for dynamic cloud computing.
- **Key Features**:
  - Can be reassigned to any instance within the same region.
  - Useful for replacing a failed instance with minimal downtime.
- **Use Case**: Assigning a stable IP address for your application.

## VPC Endpoints
VPC Endpoints enable private connections between your VPC and supported AWS services without requiring an Internet Gateway.
- **Types**:
  - **Gateway Endpoint**: For services like S3 and DynamoDB.
  - **Interface Endpoint**: For services accessed over a private network.
- **Use Case**: Securely accessing AWS services from a private subnet.

## VPC Peering
VPC Peering allows you to connect two VPCs for private communication.
- **Key Features**:
  - Supports intra-region and inter-region connections.
  - Does not support transitive routing; each VPC must be explicitly peered.
- **Use Case**: Enabling secure communication between different VPCs in multi-account architectures.
