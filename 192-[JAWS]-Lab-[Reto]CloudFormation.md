# AWS re/Start Challenge Lab - Using AWS CloudFormation to Create an AWS VPC and Amazon EC2 Instance!!!!

## Lab Overview
This lab provides an environment for creating an Amazon VPC and an Amazon EC2 instance (along with other supporting elements) using an AWS CloudFormation template. The goal of this lab is to create a CloudFormation template with the following components:

* An Amazon Virtual Private Cloud (VPC)

[![8.png](https://i.postimg.cc/5tRZNSC5/8.png)](https://postimg.cc/5H8Psvpj)

* An internet gateway attached to the VPC

[![9.png](https://i.postimg.cc/YS95Hshq/9.png)](https://postimg.cc/WdBWmSGQ)

* Security groups configured to allow SSH access to the VPC from anywhere

[![10.png](https://i.postimg.cc/d38X6MSn/10.png)](https://postimg.cc/H8kBWNm7)
  
* A private subnet within the VPC

[![11.png](https://i.postimg.cc/wMKZDxF7/11.png)](https://postimg.cc/xJR622w2)

[![12.png](https://i.postimg.cc/x10rsCQm/12.png)](https://postimg.cc/ZCDDnJCY)
  
* An Amazon EC2 instance (a T3.micro) within the private subnet (Note: Accessing the EC2 instance via SSH or Remote Desktop is not necessary for a successful solution)

[![13.png](https://i.postimg.cc/XvpzhBzr/13.png)](https://postimg.cc/FfXpkzmv)

Build and test the lab, iterating the solution until all components are successfully created. Inform the instructor when the template builds without errors so they can review the completed solution.

## This is a yaml file

        # .yaml
        # Lab VPC with Private subnet and Internet Gateway
        
        Parameters:
        
          LabVpcCidr:
            Type: String
            Default: 10.0.0.0/20
        
          PrivateSubnetCidr:
            Type: String
            Default: 10.0.0.0/24
        
          AmazonLinuxAMIID:
            Type: AWS::SSM::Parameter::Value<AWS::EC2::Image::Id>
            Default: /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2
        
        Resources:
        
          Instance:
            Type: AWS::EC2::Instance
            Properties:
              InstanceType: t3.micro
              ImageId: !Ref AmazonLinuxAMIID
              SubnetId: !Ref PrivateSubnet
              SecurityGroupIds:
                - !Ref AppSecurityGroup
              Tags:
                - Key: Name
                  Value: App Server
        
        
        ###########
        # VPC with Internet Gateway
        ###########
        
          LabVPC:
            Type: AWS::EC2::VPC
            Properties:
              CidrBlock: !Ref LabVpcCidr
              EnableDnsSupport: true
              EnableDnsHostnames: true
              Tags:
                - Key: Name
                  Value: Lab VPC
        
          IGW:
            Type: AWS::EC2::InternetGateway
            Properties:
              Tags:
                - Key: Name
                  Value: Lab IGW
        
          VPCtoIGWConnection:
            Type: AWS::EC2::VPCGatewayAttachment
            DependsOn:
              - IGW
              - LabVPC
            Properties:
              InternetGatewayId: !Ref IGW
              VpcId: !Ref LabVPC
        
        ###########
        # Private Route Table
        ###########
        
          PrivateRouteTable:
            Type: AWS::EC2::RouteTable
            DependsOn: LabVPC
            Properties:
              VpcId: !Ref LabVPC
              Tags:
                - Key: Name
                  Value: Private Route Table
        
        ###########
        # Private Subnet
        ###########
        
          PrivateSubnet:
            Type: AWS::EC2::Subnet
            DependsOn: LabVPC
            Properties:
              VpcId: !Ref LabVPC
              CidrBlock: !Ref PrivateSubnetCidr
              AvailabilityZone: !Select 
                - 0
                - !GetAZs 
                  Ref: AWS::Region
              Tags:
                - Key: Name
                  Value: Private Subnet
        
        
          PrivateRouteTableAssociation:
            Type: AWS::EC2::SubnetRouteTableAssociation
            DependsOn:
              - PrivateRouteTable
              - PrivateSubnet
            Properties:
              RouteTableId: !Ref PrivateRouteTable
              SubnetId: !Ref PrivateSubnet
        
        ###########
        # App Security Group
        ###########
        
          AppSecurityGroup:
            Type: AWS::EC2::SecurityGroup
            DependsOn: LabVPC
            Properties:
              GroupName: App
              GroupDescription: Enable access to App
              VpcId: !Ref LabVPC
              SecurityGroupIngress:
                - IpProtocol: tcp
                  FromPort: 22
                  ToPort: 22
                  CidrIp: 0.0.0.0/0
              Tags:
                - Key: Name
                  Value: App
        
        ###########
        # Outputs
        ###########
        
        Outputs:
        
          LabVPCDefaultSecurityGroup:
            Value: !Sub ${LabVPC.DefaultSecurityGroup}


## Upload file .yaml to CloudFormation

[![1.png](https://i.postimg.cc/bwYMxB0H/1.png)](https://postimg.cc/N5VpYbz5)

[![2.png](https://i.postimg.cc/vZPK3t3R/2.png)](https://postimg.cc/SnMVK8BV)

[![3.png](https://i.postimg.cc/rFN3tk4w/3.png)](https://postimg.cc/cr6XVPTp)

[![4.png](https://i.postimg.cc/pXH3TTpS/4.png)](https://postimg.cc/YjddDtNN)

[![5.png](https://i.postimg.cc/kgw0LmMp/5.png)](https://postimg.cc/MnM3jgQ7)

[![6.png](https://i.postimg.cc/vBxC1xrJ/6.png)](https://postimg.cc/DJFxHZY6)

[![7.png](https://i.postimg.cc/9XTvCTFT/7.png)](https://postimg.cc/QV86qF7x)
