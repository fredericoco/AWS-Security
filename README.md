# AWS-Security
### What is a VPC?
A virtual private cloud (VPC) is a secure, isolated private cloud hosted within a public cloud. VPC customers can run code, store data, host websites, and do anything else they could do in an ordinary private cloud, but the private cloud is hosted remotely by a public cloud provider.
### What is CIDR block?
Classless inter-domain routing (CIDR) is a set of Internet protocol standards that is used to create unique identifiers for networks and individual devices. The IP addresses allow particular information packets to be sent to specific computers.
### What is an internet gateway?
A computer that sits between different networks or applications. The gateway converts information, data or other communications from one protocol or format to another. A router may perform some of the functions of a gateway. An Internet gateway can transfer communications between an enterprise network and the Internet.
### What is a route table? 
A route table contains a set of rules, called routes, that are used to determine where network traffic from your subnet or gateway is directed. In other words, a route table tells the network packets which way they need to go to get to their desitination.
### What is subnet?
A subnet is a range of IP addresses in your VPC. You can launch AWS resources, such as EC2 instances, into a specific subnet. When you create a subnet, you specify the IPv4 CIDR block for the subnet, which is a subset of the VPC CIDR block.
### What is NACL?
An optional layer of security that acts as a firewall for controlloing traffic in and out of a subnet. Multiple subnets can be assosiated with a single network ACL, but a subnet can be associated with only one network ACL at a time.
![aws security 2](https://user-images.githubusercontent.com/39882040/153565035-f389565f-91b1-49f5-8720-a5678116fe77.PNG)


### Steps
- select region - Ireland
- Create VPC: Go to the VPC dashboard, and go to create VPC
- valid CDIR block for our VPC 10.0.0.0/16. This creates an empty VPC
- Step 2: create Internet gateway
- Step 2.1: attach the internet Gateway to our VPC(go to actions and attach to VPC, using the VPC's ID)
- Step 3: Create a public subnet( click on public subnet and choose the VPC)
- Step 3.1: associate subnet to VPC
- Step 4: Route table/s RT for public subnet
- Step 4.1: edit routes to allow access for the internet gateway (IG), add routes whole world (00000) and choose service internet gateway(the one made earlier)
- Step 4.2 associate to out public subnet
- Step 5: create a security group in our public subnet to allow required ports/traffic
- allow port 80
- allow port 3000
- https -ssl
- Subnet CIDR block for public -10.0.4.0/24
- IPV4 -IPV6 (choose 4)

## launching instances with the new VPC
- laucnh a new instance, and choose the newly created network (VPC), and the newly created Subnet. In user data write:
```
#!/bin/bash
sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get install nginx -y
sudo systemctl restart nginx
sudo systemctl enable nginx
```
- For the security group, make a new one (103a_fred_sg_vpc), allow port 80(HTTP), port 22 (SSH), port 3000 (Custom TCP)
- This should be able to get to the nginx webpage for the instances IP
-  Note: when you set up the db, you will need to only allow access to port 27017(TCP) and 22(TCP) because you don't want to be able to access the database (db) normally for security reasons.
- In order to ssh into the db from the app we need to follow several steps.
- in the app instance, cd into the .ssh folder
- Copy and paste the eng103a.pem (on note) into the eng103a.pem, now you can ssh using a command on the connect
- now do the chmod 400 eng103a.pem
- now you can ssh into the db using the standard command.

### Issues
- I had to restart the mongod, because the DB AMI was incomplete. This was annoying because I had to ssh into the db via the app instance.
<<<<<<< HEAD

# Interview Preperation
- How did you allow access to app/db?
  
  You need to include an environment variable in the code. You do this by editing the .bashrc file and putting in the IP of the database and port as a variable. Exit the file and source it. If there are any issues make sure you sourced the file. 
- What did you do to secure it?
  
  To secure it you edit the security groups of the db. You make sure you can only access it through ports relating to the webpage.

# CI/CD 