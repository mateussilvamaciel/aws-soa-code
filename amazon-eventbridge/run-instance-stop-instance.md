# Launch instance, stop instance
1. Launch an EC2 instance
3. Launch instance in US-EAST-1A
aws ec2 run-instances \
    --image-id ami-0ebfd941bbafe70c6 \
    --instance-type t2.micro \
    --subnet-id subnet-028aeb0f9a3a8c426 \
    --security-group-ids sg-0204723d5f67452ba \
    --associate-public-ip-address \
    --key-name ll
2. Stop the EC2 instance
aws ec2 stop-instances --instance-id i-XXXXXXXXXXXXXXX