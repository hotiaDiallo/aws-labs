# Implementing EFS

This lab is part of [AWS Certified Solutions Architect - Profession](https://learn.cantrill.io/p/aws-certified-solutions-architect-professional) Course by Adrian Cantrill

we will implement a simple EFS architecture within an AWS VPC

# Step 1 : Apply base infrastucture template

Apply the base cloudformation template `A4L_TWO_EFS_EC2.yaml`. this will create a basic infrastructure in us-east-1, consisting of

- A VPC using the 10.16.0.0/16 range
- 2 x Web Subnets, WebA and WebB
- 2 x App Subnets, AppA and AppB
- An Internet Gateway
- Security Groups
- Instance Role
- SSM Endpoints
- Route Tables
- Two EC2 Instances - 1 running in WebA the other in WebB

# Step 2 - Mount EFS on InstanceA

Verify there are no currently mounted EFS file systems
```
sudo ls -la /
df -k
````

To support EFS, we need to install a package

```
sudo yum -y install amazon-efs-utils
```

we are now going to mount the file system on this instance

First, we create the mount point for the EFS filesystem

```
sudo mkdir -p /efs/wp-content
```

then we mount the file system into this mountpoint

```
sudo nano /etc/fstab
```
Paste in this line at the bottom et replace the file system ID

```
file-system-id:/ /efs/wp-content efs _netdev,tls,iam 0 0
```
Next , mount the EFS file system in the mount point by running

```
sudo mount /efs/wp-content  
```
Now let's add a test file 
```
cd /efs/wp-content
sudo touch amazingtestfile.txt
sudo nano amazingtestfile.txt
```

# Step 3 - Mount EFS on InstanceB

The goal here is to mount the EFS file system and then verify that it's shared. 

After mounting the file system, we check the content from the previous EC2 instance is there

```
cd /efs/wp-content
ls -la
cat amazingtestfile.txt
```
