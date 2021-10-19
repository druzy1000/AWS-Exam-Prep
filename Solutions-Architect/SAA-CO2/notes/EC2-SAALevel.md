## **AWS Solutions Architect Notes - EC2_SAALevel**

**Private vs Public IP (IPv4)**

- IPv4 allows for 3.7 billion different addresses in the public space
- IPv6 is newer and solves problems for the Internet of Things (IoT)


**Private vs Public IP (IPv4) Fundamental Differences**

- Public IP:
  - Public IP means the machine can be identified on the internet (WWW)
  - Must be unique across the whole web (not two machines can have the same public IP)
  - Can be geo-located easily
- Private IP:
  - Private IP means the machine can only be identified on a private network only
  - The IP must be unique across the private network
  - BUT two different private networks (two companies) can have the same IPs
  - Machines connect to WWW using a NAT + internet gateway (a proxy)
  - Only a specified range of IPs can be used as private IP

**Elastic IPs**

- With an Elastic IP address, you can mask the failure of an instance or software by rapidly remapping the address to another
  instance in your account
- You can only have 5 Elastic IP in your account (you can ask AWS to increase that)
- Overall, try to avoid using Elastic IP:
  - They often reflect poor architectural decisions
  - Instead, use a random public IP and register a DNS name to it 
  - Or; as we'll see later; use a Load Balancer and don't uas a public IP

- By default, your EC2 machine comes with:
  - A private IP for internal AWS Network
  - A public IP, for the WWW.
- When we are doing SSH into our EC2 machines:
  - We can't use a private IP, because we are not in the same network
  - We can only use the public IP.
- If your machine is stopped and then started, the public IP can change