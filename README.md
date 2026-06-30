# Terraform_vpc-ec2
Creating aws vpc and ec2 using terraform 
<img width="2720" height="1680" alt="aws_vpc_architecture (1) (1)" src="https://github.com/user-attachments/assets/060f8622-3c45-4eef-9e7c-60f9309d4561" />
# Architecture overview
This Terraform configuration provisions a minimal public-facing AWS environment in us-east-1, consisting of one VPC, one public subnet, and one EC2 instance reachable over SSH and HTTP.
Resources created

VPC (aws_vpc.myvpc) — a private network with CIDR 10.0.0.0/16.
Subnet (aws_subnet.mysub) — a 10.0.1.0/24 subnet inside the VPC. It becomes "public" because of the route table association below, not because of any explicit subnet setting.
Internet Gateway (aws_internet_gateway.gw) — attaches the VPC to the public internet.
Route Table (aws_route_table.myroute) — routes all outbound traffic (0.0.0.0/0) to the internet gateway.
Route Table Association (aws_route_table_association.rta) — links the subnet to that route table, making it a public subnet.
Security Group (aws_security_group.sg) — a firewall for the instance: inbound on port 22 (SSH) and port 80 (HTTP) from anywhere, and unrestricted outbound traffic.
EC2 Instance (aws_instance.myinsta) — a t3.micro instance launched in the subnet, with a public IP assigned and the security group attached.
```
Traffic flow
```
Internet traffic enters through the internet gateway.
The route table directs that traffic into the subnet.
The security group filters which ports on the instance are reachable (22, 80).
The EC2 instance, with a public IP, becomes accessible over SSH and HTTP.

A couple of things worth flagging in the README as known limitations, since they're common review comments on this kind of setup: the security group opens SSH (22) to 0.0.0.0/0, which is fine for a demo but should normally be restricted to your own IP; and there's only one subnet/AZ, so there's no high availability.
