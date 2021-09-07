# AWS EC2 + HTTPd
A simple AWS EC2 instance (Amazon Linux 2) with Http server and starting a script in the initialisation

## Prerequisites
AWS account
AWS CLI installed locally 

## Follow the instructions:

### Create a script: vi script.txt
```
    #!/bin/bash
    # install httpd (Linux 2 version)
    yum update -y
    yum install -y httpd
    systemctl start httpd
    systemctl enable httpd
    echo "<h1>Hello World from $(hostname -f)</h1>" > /var/www/html/index.html
```

### Get your VPC id
```
    aws ec2 describe-vpcs | grep VpcId
```

### Create a security-group:
```
    aws ec2 create-security-group --group-name my-security-group-test --description "My security group" --vpc-id vpc-1800047d
```

### Add port to the security group:
```
    aws ec2 authorize-security-group-ingress --group-name my-security-group-test --protocol tcp --port 80 --cidr 0.0.0.0/0
```

### Create a AWS instance machine, type t2.micro:
```
    aws ec2 run-instances --image-id ami-0c2b8ca1dad447f8a --count 1 --instance-type t2.micro --key-name demo --security-groups my-security-group-test --user-data file://script.txt
```

### Access the http server
```
    aws ec2 describe-instances | grep PublicIpAddress
    curl http://52.91.69.216:80
```

### Delete instance
```
    code here
```

### Delete security group
```
    code here
```