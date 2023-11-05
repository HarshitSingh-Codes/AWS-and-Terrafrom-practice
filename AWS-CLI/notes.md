     aws configure
     
AWS Access Key ID [None]: AKIAQA76Z7E6GUHVLJNC
AWS Secret Access Key [None]: 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
Default region name [None]: ap-northeast-1
Default output format [None]: 

    aws sts get-caller-identity

{
    "UserId": "AIDAQA76Z7E6BBY5HUP24",
    "Account": "002144991548",
    "Arn": "arn:aws:iam::002144991548:user/harshit.singh"
}

#### Create VPC cmd:

    aws ec2 create-vpc --cidr-block 10.0.0.0/16 --tag-specifications 'ResourceType=vpc,Tags=[{Key=Name,Value=cliVPC}]'

#### Output:-
     
    harshit@harshit-IdeaPad-3-15IIL05:~$ aws ec2 create-vpc --cidr-block 10.0.0.0/16 --tag-specifications 'ResourceType=vpc,Tags=[{Key=Name,Value=MycliVPC}]'{
        "Vpc": {
            "CidrBlock": "10.0.0.0/16",
            "DhcpOptionsId": "dopt-0e90f49bef9b19369",
            "State": "pending",
            "VpcId": "vpc-0b51e6cbf23bfd99a",
            "OwnerId": "002144991548",
            "InstanceTenancy": "default",
            "Ipv6CidrBlockAssociationSet": [],
            "CidrBlockAssociationSet": [
                {
                    "AssociationId": "vpc-cidr-assoc-027e835d80b9e52f7",
                    "CidrBlock": "10.0.0.0/16",
                    "CidrBlockState": {
                        "State": "associated"
                    }
                }
            ],
            "IsDefault": false,
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "MycliVPC"
                }
            ]
        }
    }

    
#### Create Subnet cmd:

    aws ec2 create-subnet --vpc-id vpc-0b51e6cbf23bfd99a --cidr-block 10.0.1.0/24 --availability-zone ap-northeast-1a --tag-specifications 
    'ResourceType=subnet,Tags=[{Key=Name,Value=public-01}]'  

#### Output:-

    harshit@harshit-IdeaPad-3-15IIL05:~$ aws ec2 create-subnet --vpc-id vpc-0b51e6cbf23bfd99a --cidr-block 10.0.1.0/24 --availability-zone ap-northeast-1a --tag-specifications 
    'ResourceType=subnet,Tags=[{Key=Name,Value=public-01}]'
    {
        "Subnet": {
            "AvailabilityZone": "ap-northeast-1a",
            "AvailabilityZoneId": "apne1-az4",
            "AvailableIpAddressCount": 251,
            "CidrBlock": "10.0.1.0/24",
            "DefaultForAz": false,
            "MapPublicIpOnLaunch": false,
            "State": "available",
            "SubnetId": "subnet-05f3001c70ba8558d",
            "VpcId": "vpc-0b51e6cbf23bfd99a",
            "OwnerId": "002144991548",
            "AssignIpv6AddressOnCreation": false,
            "Ipv6CidrBlockAssociationSet": [],
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "public-01"
                }
            ],
            "SubnetArn": "arn:aws:ec2:ap-northeast-1:002144991548:subnet/subnet-05f3001c70ba8558d",
            "EnableDns64": false,
            "Ipv6Native": false,
            "PrivateDnsNameOptionsOnLaunch": {
                "HostnameType": "ip-name",
                "EnableResourceNameDnsARecord": false,
                "EnableResourceNameDnsAAAARecord": false
            }
        }
    }


#### Create Route table cmd:

-     aws ec2 create-route-table --vpc-id vpc-0b51e6cbf23bfd99a

-     aws ec2 create-tags --resources rtb-02625fd195c988243 --tags Key=Name,Value=publicRT

#### Output:-


      {
          "RouteTable": {
              "Associations": [],
              "PropagatingVgws": [],
              "RouteTableId": "rtb-02625fd195c988243",
              "Routes": [
                  {
                      "DestinationCidrBlock": "10.0.0.0/16",
                      "GatewayId": "local",
                      "Origin": "CreateRouteTable",
                      "State": "active"
                  }
              ],
              "Tags": [],
              "VpcId": "vpc-0b51e6cbf23bfd99a",
              "OwnerId": "002144991548"
          }
      }



#### Create Internet Gateway cmd:

-      aws ec2 create-internet-gateway 


-      aws ec2 create-tags --resources igw-03149fecf7072e173 --tags Key=Name,Value=IGW-01

#### Output:-

        --tag-specifications 'ResourceType=internet-gateway,Tags=[{Key=Name,Value=my-igw}]'
    {
        "InternetGateway": {
            "Attachments": [],
            "InternetGatewayId": "igw-03149fecf7072e173",
            "OwnerId": "002144991548",
            "Tags": []
        }
    } 



#### Associate Route table to Subnet cmd:

    aws ec2 associate-route-table --route-table-id rtb-02625fd195c988243 --subnet-id subnet-04aec136a5bdf0872 

#### Output:-

    {
        "AssociationId": "rtbassoc-0eae0d1ea889e83eb",
        "AssociationState": {
            "State": "associated"
        }
    }

#### Attach  IGW to Route Table cmd:

check that IGW is attached to your VPC if not run below command:
 -     aws ec2 attach-internet-gateway --internet-gateway-id igw-03149fecf7072e173 --vpc-id vpc-0b51e6cbf23bfd99a

then run:
 -     aws ec2 create-route --route-table-id rtb-02625fd195c988243 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-03149fecf7072e173

#### Output:-

    {
        "Return": true
    }


#### Create Security Group cmd:

-      aws ec2 create-security-group --group-name my-cli-openSG --description "My CLI security group" --vpc-id vpc-0b51e6cbf23bfd99a

#### Output:-

    {
        "GroupId": "sg-066ea75dbd9ccd1c9"
    }

  allow all cmd

-     aws ec2 authorize-security-group-ingress --group-id sg-066ea75dbd9ccd1c9 --protocol all --cidr 0.0.0.0/0

#### Output:-

    {
        "Return": true,
        "SecurityGroupRules": [
            {
                "SecurityGroupRuleId": "sgr-05d0710a00eadcdeb",
                "GroupId": "sg-066ea75dbd9ccd1c9",
                "GroupOwnerId": "002144991548",
                "IsEgress": false,
                "IpProtocol": "-1",
                "FromPort": -1,
                "ToPort": -1,
                "CidrIpv4": "0.0.0.0/0"
            }
        ]
    }

#### Create Subnet cmd:

    aws ec2 run-instances --image-id ami-09a81b370b76de6a2 --count 1 --instance-type t2.micro --key-name AWS-cli-KeyPair --security-group-ids sg-066ea75dbd9ccd1c9 --subnet-id subnet-05f3001c70ba8558d --associate-public-ip-address
    
give a name to your instance

    aws ec2 create-tags --resources i-0f862cace445a9dd6 --tags Key=Name,Value=cli-01


ami id- ami-09a81b370b76de6a2

#### Output:-

    {
        "Groups": [],
        "Instances": [
            {
                "AmiLaunchIndex": 0,
                "ImageId": "ami-09a81b370b76de6a2",
                "InstanceId": "i-0f862cace445a9dd6",
                "InstanceType": "t2.micro",
                "KeyName": "AWS-cli-KeyPair",
                "LaunchTime": "2023-11-05T17:36:59+00:00",
                "Monitoring": {
                    "State": "disabled"
                },
                "Placement": {
                    "AvailabilityZone": "ap-northeast-1a",
                    "GroupName": "",
                    "Tenancy": "default"
                },
                "PrivateDnsName": "ip-10-0-1-242.ap-northeast-1.compute.internal",
                "PrivateIpAddress": "10.0.1.242",
                "ProductCodes": [],
                "PublicDnsName": "",
                "State": {
                    "Code": 0,
                    "Name": "pending"
                },
                "StateTransitionReason": "",
                "SubnetId": "subnet-05f3001c70ba8558d",
                "VpcId": "vpc-0b51e6cbf23bfd99a",
                "Architecture": "x86_64",
                "BlockDeviceMappings": [],
                "ClientToken": "fc1b51d8-e261-4dde-8bc0-924b729abd61",
                "EbsOptimized": false,
                "EnaSupport": true,
                "Hypervisor": "xen",
                "NetworkInterfaces": [
                    {
                        "Attachment": {
                            "AttachTime": "2023-11-05T17:36:59+00:00",
                            "AttachmentId": "eni-attach-0924cd7eec6946680",
                            "DeleteOnTermination": true,
                            "DeviceIndex": 0,

#### Create an application load balancer cmd:

    aws elbv2 create-load-balancer --name cli-ALB  --subnets subnet-05f3001c70ba8558d subnet-04aec136a5bdf0872 --security-groups sg-066ea75dbd9ccd1c9 --type application

#### Create Target Group cmd:

    aws elbv2 create-target-group     --name cli-TG     --protocol HTTP     --port 80     --target-type instance     --vpc-id  vpc-0b51e6cbf23bfd99a

#### Register targets

    aws elbv2 register-targets --target-group-arn arn:aws:elasticloadbalancing:ap-northeast-1:002144991548:targetgroup/cli-TG/25dd323eba87a690  --targets Id=i-03d0e60a36bb2d679 Id=i-00c64d8f9bfa1b61d

#### Create Listener

    aws elbv2 create-listener --load-balancer-arn arn:aws:elasticloadbalancing:ap-northeast-1:002144991548:loadbalancer/app/cli-ALB/7059ba205f5d320f --protocol HTTP --port 80 --default-actions Type=forward,TargetGroupArn=arn:aws:elasticloadbalancing:ap-northeast-1:002144991548:targetgroup/cli-TG/25dd323eba87a690
