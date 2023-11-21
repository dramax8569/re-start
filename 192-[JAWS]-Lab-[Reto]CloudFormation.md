# AWS re/Start Challenge Lab - Using AWS CloudFormation to Create an AWS VPC and Amazon EC2 Instance

## Lab Overview
This lab provides an environment for creating an Amazon VPC and an Amazon EC2 instance (along with other supporting elements) using an AWS CloudFormation template. The goal of this lab is to create a CloudFormation template with the following components:
* An Amazon Virtual Private Cloud (VPC)
* An internet gateway attached to the VPC
* Security groups configured to allow SSH access to the VPC from anywhere
* A private subnet within the VPC
* An Amazon EC2 instance (a T3.micro) within the private subnet (Note: Accessing the EC2 instance via SSH or Remote Desktop is not necessary for a successful solution)

Build and test the lab, iterating the solution until all components are successfully created. Inform the instructor when the template builds without errors so they can review the completed solution.

    ```
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
    
    ```
