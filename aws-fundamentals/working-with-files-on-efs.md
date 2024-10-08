# Working with Files on EFS

## Launch EC2 instances
1. Create a security group
aws ec2 create-security-group --group-name EFS-Lab --description "Temporary SG for the EC2 and EBS Lab" --vpc-id vpc-083afff6af325ccd4
2. Add a rule for SSH inbound to the security group
aws ec2 authorize-security-group-ingress --group-id sg-034ef036fbb4e206c --protocol tcp --port 22 --cidr 0.0.0.0/0
3. Add rule to the security group to allow the NFS protocol from group members
aws ec2 authorize-security-group-ingress --group-id sg-034ef036fbb4e206c --protocol tcp --port 2049 --source-group sg-034ef036fbb4e206c
4. Launch instance in US-EAST-1A
aws ec2 run-instances \
    --image-id ami-0ebfd941bbafe70c6 \
    --instance-type t2.micro \
    --placement AvailabilityZone=us-east-1a \
    --subnet-id subnet-028aeb0f9a3a8c426 \
    --security-group-ids sg-034ef036fbb4e206c \
    --associate-public-ip-address \
    --key-name ll
5. Launch instance in US-EAST-1B
aws ec2 run-instances \
    --image-id ami-0ebfd941bbafe70c6 \
    --instance-type t2.micro \
    --placement AvailabilityZone=us-east-1b \
    --subnet-id subnet-091edb077c392e60f \
    --security-group-ids sg-034ef036fbb4e206c \
    --associate-public-ip-address \
    --key-name ll

## Create an EFS File System
1. Create an EFS file system (console, and add the EFS-Lab security group to the mount targets for each AZ)

## Mount using the NFS Client (perform steps on both instances)
1. Create an EFS mount point
mkdir ~/efs-mount-point
2. Install NFS client
sudo yum -y install nfs-utils
3. Mount using the EFS client
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport 10.0.1.176:/ efs

4. Create a file on the file system
5. Add a file system policy to enforce encryption in-transit
6. Unmount (make sure to change directory out of efs-mount-point first)
sudo umount ~/efs-mount-point

## Mount using the EFS utils (perform steps on both instances)
1. Install EFS utils
sudo dnf install amazon-efs-utils
Sem esse pacote da o erro
mount: /efs: unknown filesystem type 'efs'.
2. Mount using the EFS mount helper
sudo mount -t efs -o tls,mounttargetip=10.0.1.176 fs-0a4a5690a68f7231c efs/
sudo mount -t efs -o tls,mounttargetip=10.0.2.46 fs-0a4a5690a68f7231c efs/

