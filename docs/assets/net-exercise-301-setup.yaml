AWSTemplateFormatVersion: "2010-09-09"
Description: "3 VPCs in 2 AZs  with Ec2 instances for webserver, openswan connectivity, bind instance and 3 testing instances"

Metadata:
  "AWS::CloudFormation::Interface":
    ParameterGroups:
      - Label:
          default: "VPC Parameters"
        Parameters:
          - AvailabilityZoneA
          - AvailabilityZoneB

Parameters:
  AvailabilityZoneA:
    Description: Availability Zone 1
    Type: AWS::EC2::AvailabilityZone::Name
    Default: eu-west-1a
  AvailabilityZoneB:
    Description: Availability Zone 2
    Type: AWS::EC2::AvailabilityZone::Name
    Default: eu-west-1b

Resources:
  WebserverVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: "10.0.0.0/16"
      EnableDnsSupport: "true"
      EnableDnsHostnames: "true"
      InstanceTenancy: default
      Tags:
        - Key: Name
          Value: "WebserverVPC"

  PublicSubnetWebserverVPC:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId:
        Ref: WebserverVPC
      CidrBlock: "10.0.3.0/24"
      AvailabilityZone:
        Ref: AvailabilityZoneA
      Tags:
        - Key: Name
          Value: "PublicSubnetWebserverVPC"

  PrivateSubnetWebserverVPC:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId:
        Ref: WebserverVPC
      CidrBlock: "10.0.4.0/24"
      AvailabilityZone:
        Ref: AvailabilityZoneB
      Tags:
        - Key: Name
          Value: "PrivateSubnetWebserverVPC"

  WebserverVPCPublicRT:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId:
        Ref: WebserverVPC
      Tags:
        - Key: Name
          Value: "WebserverVPCPublicRT"

  WebserverVPCPrivateRT:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId:
        Ref: WebserverVPC
      Tags:
        - Key: Name
          Value: "WebserverVPCPrivateRT"

  PublicWebserverRTAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      RouteTableId:
        Ref: WebserverVPCPublicRT
      SubnetId:
        Ref: PublicSubnetWebserverVPC

  PrivateSubnetWebserverRTAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      RouteTableId:
        Ref: WebserverVPCPrivateRT
      SubnetId:
        Ref: PrivateSubnetWebserverVPC

  WebServerVPCIGW:
    Type: AWS::EC2::InternetGateway
    Properties:
      Tags:
        - Key: Name
          Value: WebServerVPCIGW

  WebServerVPCIGWAttach:
    Type: AWS::EC2::VPCGatewayAttachment
    Properties:
      VpcId:
        Ref: WebserverVPC
      InternetGatewayId:
        Ref: WebServerVPCIGW

  WebServerVPCDirectInternetRoute:
    Type: AWS::EC2::Route
    DependsOn: WebServerVPCIGW
    Properties:
      DestinationCidrBlock: 0.0.0.0/0
      GatewayId:
        Ref: WebServerVPCIGW
      RouteTableId:
        Ref: WebserverVPCPublicRT

  WebServerVPCNATEIP:
    Type: "AWS::EC2::EIP"
    Properties:
      Domain: vpc

  WebServerVPCNATGW:
    DependsOn: WebServerVPCIGWAttach
    Type: "AWS::EC2::NatGateway"
    Properties:
      AllocationId:
        Fn::GetAtt:
          - WebServerVPCNATEIP
          - AllocationId
      SubnetId:
        Ref: PublicSubnetWebserverVPC
      Tags:
        - Key: Name
          Value: WebServerVPCNATGW

  WebServerVPCNATGWRouteAttach:
    Type: AWS::EC2::Route
    DependsOn: WebServerVPCIGW
    Properties:
      DestinationCidrBlock: 0.0.0.0/0
      NatGatewayId:
        Ref: WebServerVPCNATGW
      RouteTableId:
        Ref: WebserverVPCPrivateRT

  ServicesVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: "10.0.0.0/16"
      EnableDnsSupport: "true"
      EnableDnsHostnames: "true"
      InstanceTenancy: default
      Tags:
        - Key: Name
          Value: "ServicesVPC"

  PublicSubnetServicesVPC:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId:
        Ref: ServicesVPC
      CidrBlock: "10.0.1.0/24"
      AvailabilityZone:
        Ref: AvailabilityZoneA
      Tags:
        - Key: Name
          Value: "PublicSubnetServicesVPC"

  PrivateSubnetServicesVPC:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId:
        Ref: ServicesVPC
      CidrBlock: "10.0.2.0/24"
      AvailabilityZone:
        Ref: AvailabilityZoneB
      Tags:
        - Key: Name
          Value: "PrivateServicesVPC"

  ServicesVPCPublicRT:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId:
        Ref: ServicesVPC
      Tags:
        - Key: Name
          Value: "ServicesVPCPublicRT"

  ServicesVPCPrivateRT:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId:
        Ref: ServicesVPC
      Tags:
        - Key: Name
          Value: "ServicesVPCPrivateRT"          


  PublicSubnetServicesRTAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      RouteTableId:
        Ref: ServicesVPCPublicRT
      SubnetId:
        Ref: PublicSubnetServicesVPC

  PrivateSubnetServicesRTAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      RouteTableId:
        Ref: ServicesVPCPrivateRT
      SubnetId:
        Ref: PrivateSubnetServicesVPC

  ServicesVPCIGW:
    Type: AWS::EC2::InternetGateway
    Properties:
      Tags:
        - Key: Name
          Value: ServicesVPCIGW

  ServicesVPCIGWAttach:
    Type: AWS::EC2::VPCGatewayAttachment
    Properties:
      VpcId:
        Ref: ServicesVPC
      InternetGatewayId:
        Ref: ServicesVPCIGW

  ServicesVPCDirectInternetRoute:
    Type: AWS::EC2::Route
    DependsOn: ServicesVPCIGW
    Properties:
      DestinationCidrBlock: 0.0.0.0/0
      GatewayId:
        Ref: ServicesVPCIGW
      RouteTableId:
        Ref: ServicesVPCPublicRT

  ServicesVPCNATEIP:
    Type: "AWS::EC2::EIP"
    Properties:
      Domain: vpc

  ServicesVPCNATGW:
    DependsOn: ServicesVPCIGWAttach
    Type: "AWS::EC2::NatGateway"
    Properties:
      AllocationId:
        Fn::GetAtt:
          - ServicesVPCNATEIP
          - AllocationId
      SubnetId:
        Ref: PublicSubnetServicesVPC
      Tags:
        - Key: Name
          Value: ServicesVPCNATGW

  ServicesVPCNATGWRouteAttach:
    Type: AWS::EC2::Route
    DependsOn: ServicesVPCIGW
    Properties:
      DestinationCidrBlock: 0.0.0.0/0
      NatGatewayId:
        Ref: ServicesVPCNATGW
      RouteTableId:
        Ref: ServicesVPCPrivateRT

  DCVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: "10.2.0.0/16"
      EnableDnsSupport: "true"
      EnableDnsHostnames: "true"
      InstanceTenancy: default
      Tags:
        - Key: Name
          Value: "DCVPC"

  PublicSubnetDCVPC:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId:
        Ref: DCVPC
      CidrBlock: "10.2.1.0/24"
      AvailabilityZone:
        Ref: AvailabilityZoneA
      Tags:
        - Key: Name
          Value: "PublicSubnetDCVPC"

  PrivateSubnetDCVPC:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId:
        Ref: DCVPC
      CidrBlock: "10.2.2.0/24"
      AvailabilityZone:
        Ref: AvailabilityZoneB
      Tags:
        - Key: Name
          Value: "PrivateSubnetDCVPC"

  DCVPCRTPublic:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId:
        Ref: DCVPC
      Tags:
        - Key: Name
          Value: "DCVPCRTPublic"

  DCVPCRTPrivate:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId:
        Ref: DCVPC
      Tags:
        - Key: Name
          Value: "DCVPCRTPrivate"

  PublicSubnetDCRTAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      RouteTableId:
        Ref: DCVPCRTPublic
      SubnetId:
        Ref: PublicSubnetDCVPC

  PrivateSubnetDCRTAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      RouteTableId:
        Ref: DCVPCRTPrivate
      SubnetId:
        Ref: PrivateSubnetDCVPC

  InternetGatewayDCVPC:
    Type: AWS::EC2::InternetGateway
    Properties:
      Tags:
        - Key: Name
          Value: IGWDCVPC

  AttachGatewayDCVPC:
    Type: AWS::EC2::VPCGatewayAttachment
    Properties:
      VpcId:
        Ref: DCVPC
      InternetGatewayId:
        Ref: InternetGatewayDCVPC

  DirectInternetRouteDCVPC:
    Type: AWS::EC2::Route
    DependsOn: InternetGatewayDCVPC
    Properties:
      DestinationCidrBlock: 0.0.0.0/0
      GatewayId:
        Ref: InternetGatewayDCVPC
      RouteTableId:
        Ref: DCVPCRTPublic

  DCVPCNATEIP:
    Type: "AWS::EC2::EIP"
    Properties:
      Domain: vpc

  DCVPCNATGW:
    DependsOn: AttachGatewayDCVPC
    Type: "AWS::EC2::NatGateway"
    Properties:
      AllocationId:
        Fn::GetAtt:
          - DCVPCNATEIP
          - AllocationId
      SubnetId:
        Ref: PublicSubnetDCVPC
      Tags:
        - Key: Name
          Value: DCPCNATGW

  DCVPCNATGWRouteAttach:
    Type: AWS::EC2::Route
    DependsOn: InternetGatewayDCVPC
    Properties:
      DestinationCidrBlock: 0.0.0.0/0
      NatGatewayId:
        Ref: DCVPCNATGW
      RouteTableId:
        Ref: DCVPCRTPrivate

  EC2Role:
    Type: AWS::IAM::Role
    Properties:
      RoleName: "SSMAccessToInstances"
      Path: "/"
      ManagedPolicyArns:
        - "arn:aws:iam::aws:policy/service-role/AmazonEC2RoleforSSM"
        - "arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore"
        - "arn:aws:iam::aws:policy/AdministratorAccess"
      AssumeRolePolicyDocument:
        Version: "2012-10-17"
        Statement:
          - Effect: Allow
            Principal: 
              Service:
                - ec2.amazonaws.com
            Action:
              - sts:AssumeRole

  Ec2SecurityGroupWebserverVPC:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Open-up ports for ICMP and TCP from 10.x.x.x
      GroupName: WebVPC-EC2-Security-Group
      VpcId:
        Ref: WebserverVPC
      SecurityGroupIngress:
        - IpProtocol: icmp
          CidrIp: 10.0.0.0/8
          FromPort: "-1"
          ToPort: "-1"
        - IpProtocol: tcp
          CidrIp: 10.0.0.0/8
          FromPort: "1"
          ToPort: "65535"
        - IpProtocol: udp
          CidrIp: 10.0.0.0/8
          FromPort: "1"
          ToPort: "65535"
      Tags:
        - Key: Name
          Value: WebVPC-EC2-Security-Group

  Ec2SecurityGroupServicesVPC:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Open-up ports for ICMP and TCP from 10.x.x.x
      GroupName: ServicesVPC-EC2-Security-Group
      VpcId:
        Ref: ServicesVPC
      SecurityGroupIngress:
        - IpProtocol: icmp
          CidrIp: 10.0.0.0/8
          FromPort: "-1"
          ToPort: "-1"
        - IpProtocol: tcp
          CidrIp: 10.0.0.0/8
          FromPort: "1"
          ToPort: "65535"
        - IpProtocol: udp
          CidrIp: 10.0.0.0/8
          FromPort: "1"
          ToPort: "65535"
      Tags:
        - Key: Name
          Value: ServicesVPC-EC2-Security-Group

  Ec2SecurityGroupDCVPC:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Open-up ports for ICMP and TCP from 10.x.x.x
      GroupName: DCVPC-EC2-Security-Group
      VpcId:
        Ref: DCVPC
      SecurityGroupIngress:
        - IpProtocol: icmp
          CidrIp: 10.0.0.0/8
          FromPort: "-1"
          ToPort: "-1"
        - IpProtocol: tcp
          CidrIp: 10.0.0.0/8
          FromPort: "1"
          ToPort: "65535"
        - IpProtocol: udp
          CidrIp: 10.0.0.0/8
          FromPort: "1"
          ToPort: "65535"
      Tags:
        - Key: Name
          Value: DCVPC-EC2-Security-Group

  Ec2SecurityGroupOpenSwan:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: OpenSwan instance security group
      GroupName: OpenSwan-SG
      VpcId:
        Ref: DCVPC
      SecurityGroupIngress:
        - IpProtocol: icmp
          CidrIp: 10.0.0.0/8
          FromPort: "-1"
          ToPort: "-1"
        - IpProtocol: tcp
          CidrIp: 10.0.0.0/8
          FromPort: "1"
          ToPort: "65535"
        - IpProtocol: udp
          CidrIp: 10.0.0.0/8
          FromPort: "1"
          ToPort: "65535"
        - IpProtocol: udp
          CidrIp: 0.0.0.0/0
          FromPort: "4500"
          ToPort: "4500"
        - IpProtocol: udp
          CidrIp: "0.0.0.0/0"
          FromPort: "500"
          ToPort: "500"
      Tags:
        - Key: Name
          Value: OpenSwan-SG

  WebVPCSSMMEndpoint:
    Type: AWS::EC2::VPCEndpoint
    Properties:
      VpcId:
        Ref: WebserverVPC
      ServiceName: !Sub "com.amazonaws.${AWS::Region}.ssmmessages"
      VpcEndpointType: Interface
      PrivateDnsEnabled: true
      SubnetIds:
        - Ref: PublicSubnetWebserverVPC
        - Ref: PrivateSubnetWebserverVPC
      SecurityGroupIds:
        - Ref: Ec2SecurityGroupWebserverVPC

  WebVPCEc2MessagesEndpoint:
    Type: AWS::EC2::VPCEndpoint
    Properties:
      VpcId:
        Ref: WebserverVPC
      ServiceName: !Sub "com.amazonaws.${AWS::Region}.ec2messages"
      VpcEndpointType: Interface
      PrivateDnsEnabled: true
      SubnetIds:
        - Ref: PublicSubnetWebserverVPC
        - Ref: PrivateSubnetWebserverVPC
      SecurityGroupIds:
        - Ref: Ec2SecurityGroupWebserverVPC


  WebVPCSSMEndpoint:
    Type: AWS::EC2::VPCEndpoint
    Properties:
      VpcId:
        Ref: WebserverVPC
      ServiceName: !Sub "com.amazonaws.${AWS::Region}.ssm"
      VpcEndpointType: Interface
      PrivateDnsEnabled: true
      SubnetIds:
        - Ref: PublicSubnetWebserverVPC
        - Ref: PrivateSubnetWebserverVPC
      SecurityGroupIds:
        - Ref: Ec2SecurityGroupWebserverVPC

  ServicesVPCSSMMEndpoint:
    Type: AWS::EC2::VPCEndpoint
    Properties:
      VpcId:
        Ref: ServicesVPC
      ServiceName: !Sub "com.amazonaws.${AWS::Region}.ssmmessages"
      VpcEndpointType: Interface
      PrivateDnsEnabled: true
      SubnetIds:
        - Ref: PublicSubnetServicesVPC
        - Ref: PrivateSubnetServicesVPC
      SecurityGroupIds:
        - Ref: Ec2SecurityGroupServicesVPC

  ServicesVPCEc2MessagesEndpoint:
    Type: AWS::EC2::VPCEndpoint
    Properties:
      VpcId:
        Ref: ServicesVPC
      ServiceName: !Sub "com.amazonaws.${AWS::Region}.ec2messages"
      VpcEndpointType: Interface
      PrivateDnsEnabled: true
      SubnetIds:
        - Ref: PublicSubnetServicesVPC
        - Ref: PrivateSubnetServicesVPC
      SecurityGroupIds:
        - Ref: Ec2SecurityGroupServicesVPC

  ServicesVPCSSMEndpoint:
    Type: AWS::EC2::VPCEndpoint
    Properties:
      VpcId:
        Ref: ServicesVPC
      ServiceName: !Sub "com.amazonaws.${AWS::Region}.ssm"
      VpcEndpointType: Interface
      PrivateDnsEnabled: true
      SubnetIds:
        - Ref: PublicSubnetServicesVPC
        - Ref: PrivateSubnetServicesVPC
      SecurityGroupIds:
        - Ref: Ec2SecurityGroupServicesVPC

  DCVPCSSMMEndpoint:
    Type: AWS::EC2::VPCEndpoint
    Properties:
      VpcId:
        Ref: DCVPC
      ServiceName: !Sub "com.amazonaws.${AWS::Region}.ssmmessages"
      VpcEndpointType: Interface
      PrivateDnsEnabled: true
      SubnetIds:
        - Ref: PublicSubnetDCVPC
        - Ref: PrivateSubnetDCVPC
      SecurityGroupIds:
        - Ref: Ec2SecurityGroupDCVPC

  DCVPCEc2MessagesEndpoint:
    Type: AWS::EC2::VPCEndpoint
    Properties:
      VpcId:
        Ref: DCVPC
      ServiceName: !Sub "com.amazonaws.${AWS::Region}.ec2messages"
      VpcEndpointType: Interface
      PrivateDnsEnabled: true
      SubnetIds:
        - Ref: PublicSubnetDCVPC
        - Ref: PrivateSubnetDCVPC
      SecurityGroupIds:
        - Ref: Ec2SecurityGroupDCVPC


  DCVPCSSMEndpoint:
    Type: AWS::EC2::VPCEndpoint
    Properties:
      VpcId:
        Ref: DCVPC
      ServiceName: !Sub "com.amazonaws.${AWS::Region}.ssm"
      VpcEndpointType: Interface
      PrivateDnsEnabled: true
      SubnetIds:
        - Ref: PublicSubnetDCVPC
        - Ref: PrivateSubnetDCVPC
      SecurityGroupIds:
        - Ref: Ec2SecurityGroupDCVPC

  InstanceProfileEC2:
    Type: AWS::IAM::InstanceProfile
    DependsOn:
      - EC2Role
    Properties:
      Path: "/"
      Roles:
        - SSMAccessToInstances

  WebServerInstance:
    Type: AWS::EC2::Instance
    DependsOn: 
      - PrivateSubnetWebserverVPC
      - EC2Role
      - Ec2SecurityGroupWebserverVPC
    Properties:
      UserData:
        Fn::Base64:
            !Sub |
              #!/bin/bash
              /usr/bin/yum -y install httpd php
              /sbin/chkconfig httpd on
              /sbin/service httpd start
              /bin/echo -n "Welcome to my web server. Server private IP is " > /var/www/html/index.php
              /opt/aws/bin/ec2-metadata -o | /bin/cut -d" " -f2 >> /var/www/html/index.php
              /bin/echo -n "Availability Zone: " >> /var/www/html/index.php
              /opt/aws/bin/ec2-metadata -z | /bin/cut -d" " -f2 >> /var/www/html/index.php
              /bin/echo "your ip is <?php \$ip = isset(\$_SERVER['HTTP_CLIENT_IP']) ? \$_SERVER['HTTP_CLIENT_IP'] : isset(\$_SERVER['HTTP_X_FORWARDED_FOR']) ? \$_SERVER['HTTP_X_FORWARDED_FOR'] : \$_SERVER['REMOTE_ADDR']; echo \$ip;?>" >> /var/www/html/index.php
              /bin/echo "" >> /var/www/html/index.php
              /bin/echo "LoadModule remoteip_module modules/mod_remoteip.so" > /etc/httpd/conf.d/remoteip.conf
              /bin/echo "RemoteIPHeader X-Forwarded-For" >> /etc/httpd/conf.d/remoteip.conf
              /bin/echo "RemoteIPInternalProxy 100.64.0.0/10" >> /etc/httpd/conf.d/remoteip.conf
              /bin/echo "RemoteIPProxyProtocol Off" >> /etc/httpd/conf/httpd.conf
              /bin/echo "RemoteIPProxyProtocolExceptions 127.0.0.1 100.64.0.0/10" >> /etc/httpd/conf/httpd.conf
              /sbin/service httpd restart
              systemctl enable amazon-ssm-agent
              systemctl start amazon-ssm-agent

      SubnetId:
        Ref: PrivateSubnetWebserverVPC
      ImageId: ami-0713f98de93617bb4
      InstanceType: t2.micro
      PrivateIpAddress: 10.0.4.100
      SecurityGroupIds:
        - Ref: Ec2SecurityGroupWebserverVPC
      IamInstanceProfile:
        Ref: InstanceProfileEC2
      Tags:
        - Key: Name
          Value: WebServerInstance

  ServicesInstance:
    Type: AWS::EC2::Instance
    DependsOn: 
      - PrivateSubnetServicesVPC
      - EC2Role
      - Ec2SecurityGroupServicesVPC
    Properties:
      UserData:
        Fn::Base64:
            !Sub |
              #!/bin/bash
              /bin/echo -n "app1.aws.example.com" > /etc/hostname
              /bin/hostname $(cat /etc/hostname)
              /bin/echo -e "127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4 app1 app1.aws.example.com\n::1         localhost6 localhost6.localdomain6" > /etc/hosts
              systemctl enable amazon-ssm-agent
              systemctl start amazon-ssm-agent

      SubnetId:
        Ref: PrivateSubnetServicesVPC
      ImageId: ami-0713f98de93617bb4
      InstanceType: t2.micro
      PrivateIpAddress: 10.0.2.100
      SecurityGroupIds:
        - Ref: Ec2SecurityGroupServicesVPC
      IamInstanceProfile:
        Ref: InstanceProfileEC2
      Tags:
        - Key: Name
          Value: ServicesAppInstance

  DCAppInstance:
    Type: AWS::EC2::Instance
    DependsOn: 
      - PrivateSubnetDCVPC
      - EC2Role
      - Ec2SecurityGroupDCVPC
    Properties:
      UserData:
        Fn::Base64:
            !Sub |
              #!/bin/bash
              /bin/echo -n "dc1.example.com" > /etc/hostname
              /bin/hostname $(cat /etc/hostname)
              /bin/echo -e "127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4 dc1 dc1.example.com\n::1         localhost6 localhost6.localdomain6" > /etc/hosts
              systemctl enable amazon-ssm-agent
              systemctl start amazon-ssm-agent

      SubnetId:
        Ref: PrivateSubnetDCVPC
      ImageId: ami-0713f98de93617bb4
      InstanceType: t2.micro
      PrivateIpAddress: 10.2.2.100
      SecurityGroupIds:
        - Ref: Ec2SecurityGroupDCVPC
      IamInstanceProfile:
        Ref: InstanceProfileEC2
      Tags:
        - Key: Name
          Value: DCInstance

  DCBindInstance:
    Type: AWS::EC2::Instance
    DependsOn: 
      - PrivateSubnetDCVPC
      - EC2Role
      - Ec2SecurityGroupDCVPC
    Properties:
      UserData:
        Fn::Base64:
            !Sub |
              #!/bin/bash
              /bin/echo -n "ns1.example.com" > /etc/hostname
              /bin/hostname $(cat /etc/hostname)
              /bin/echo -e "127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4 ns1 ns1.example.com\n::1         localhost6 localhost6.localdomain6" > /etc/hosts
              systemctl enable amazon-ssm-agent
              systemctl start amazon-ssm-agent
              yum install -y bind bind-libs bind-utils
              chkconfig named on
              cp  /etc/named.conf /etc/named.conf.Bk
              echo >  /etc/named.conf
              cat << 'EOF' >> /etc/named.conf
              options {
               directory "/var/named";
               version "not currently available";
              # Listen to the loopback device only
               listen-on { any; };
               listen-on-v6 { ::1; };
              # Do not query from the specified source port range
              # (Adjust depending your firewall configuration)
               avoid-v4-udp-ports { range 1 32767; };
               avoid-v6-udp-ports { range 1 32767; };
              # Forward all DNS queries to the Google Public DNS.
               forwarders { 8.8.8.8;4.2.2.5; };
              # forward only;
              # Expire negative answer ASAP.
              # i.e. Do not cache DNS query failure.
               max-ncache-ttl 1200; # 3 seconds
              # Disable non-relevant operations
               allow-transfer { none; };
               allow-update-forwarding { none; };
               allow-notify { none; };
               allow-recursion { any; };
              };
              zone "example.com" in {
              type master;
              file "/etc/named/db.example.com";
              allow-update { none; };
              };
              zone "aws.example.com" in {
              type forward;
              forwarders { 10.0.1.11; 10.0.2.11; };
              };
              EOF
              echo >  /etc/named/db.example.com
              cat << 'EOF' >> /etc/named/db.example.com
              $TTL 86400
              @       IN      SOA     example.com. admin.example.com. (
                          3         ; Serial
                          604800     ; Refresh
                          86400     ; Retry
                          2419200     ; Expire
                          604800 )   ; Negative Cache TTL
                      
                      ; name servers - NS records
              @        IN      NS      ns1.example.com.
              
              ; name servers - A records
              ns1          IN      A       10.2.2.53

              ; other servers
              dc1         IN      A      10.2.2.100
              
              $ORIGIN aws.example.com.
              @       IN      NS      ep1.aws.example.com.
              
                      IN      NS      ep2.aws.example.com.
              
              ep1 IN      A       10.0.1.11
              ep2 IN      A       10.0.2.11
              EOF
              systemctl start named
              


      SubnetId:
        Ref: PrivateSubnetDCVPC
      ImageId: ami-0713f98de93617bb4
      InstanceType: t2.micro
      PrivateIpAddress: 10.2.2.53
      SecurityGroupIds:
        - Ref: Ec2SecurityGroupDCVPC
      IamInstanceProfile:
        Ref: InstanceProfileEC2
      Tags:
        - Key: Name
          Value: DCBindInstance

  OpenSwanEIP:
    Type: "AWS::EC2::EIP"
    Properties:
      Domain: vpc
  
  OpenSwanENI:
    Type: AWS::EC2::NetworkInterface
    Properties:
      SubnetId:
        Ref: PublicSubnetDCVPC
      Description: Interface for openswan instance
      GroupSet:
        - Ref: Ec2SecurityGroupOpenSwan
      SourceDestCheck: false
      Tags:
        - Key: Name
          Value: OpenSwanENI

  AssociateOpenSwanEIP:
    Type: AWS::EC2::EIPAssociation
    DependsOn:
      - OpenSwanEIP
      - OpenSwanENI
    Properties:
      AllocationId: !GetAtt OpenSwanEIP.AllocationId
      NetworkInterfaceId: !Ref OpenSwanENI

  DCOpenSwanInstance:
    Type: AWS::EC2::Instance
    DependsOn: 
      - PublicSubnetDCVPC
      - EC2Role
      - OpenSwanENI
    Properties:
      UserData:
        Fn::Base64:
            !Sub |
              #!/bin/bash
              systemctl enable amazon-ssm-agent
              systemctl start amazon-ssm-agent
              yum install -y openswan
              echo -e "net.ipv4.ip_forward = 1" >>  /etc/sysctl.conf
              sysctl -p /etc/sysctl.conf -- bring change into effect
              service network restart
              echo 0 | sudo dd of=/proc/sys/net/ipv4/conf/all/send_redirects
              echo 0 | sudo dd of=/proc/sys/net/ipv4/conf/all/send_redirects
              echo 0 | sudo dd of=/proc/sys/net/ipv4/conf/*/send_redirects
              echo 0 | sudo dd of=/proc/sys/net/ipv4/conf/default/send_redirects
              echo 0 | sudo dd of=/proc/sys/net/ipv4/conf/eth0/send_redirects
              echo 0 | sudo dd of=/proc/sys/net/ipv4/conf/lo0/send_redirects
              echo 0 | sudo dd of=/proc/sys/net/ipv4/conf/lo/send_redirects
              echo 0 | sudo dd of=/proc/sys/net/ipv4/conf/ip_vti0/send_redirects
              echo 0 | sudo dd of=/proc/sys/net/ipv4/conf/ip_vti0/accept_redirects
              echo 0 | sudo dd of=/proc/sys/net/ipv4/conf/all/accept_redirects
              echo 0 | sudo dd of=/proc/sys/net/ipv4/conf/default/accept_redirects
              echo 0 | sudo dd of=/proc/sys/net/ipv4/conf/lo/accept_redirects
              echo 0 | sudo dd of=/proc/sys/net/ipv4/conf/eth0/accept_redirects
              echo 0 | sudo dd of=/proc/sys/net/ipv4/conf/all/rp_filter
              echo 0 | sudo dd of=/proc/sys/net/ipv4/conf/default/rp_filter  
              echo 0 | sudo dd of=/proc/sys/net/ipv4/conf/eth0/rp_filter  
              echo 0 | sudo dd of=/proc/sys/net/ipv4/conf/ip_vti0/rp_filter
              cat <<'EOF' >> connection-setup.sh
              #!/bin/bash
              EIP=$(curl http://169.254.169.254/latest/meta-data/public-ipv4);
              echo -e "$EIP";
              echo -e "$1";
              echo -e "
              conn Tunnel1\n\tauthby=secret\n\tauto=start\n\tleft=%defaultroute\n\tleftid=$EIP\n\tright=$1\n\ttype=tunnel\n\tikelifetime=8h\n\tkeylife=1h\n\tphase2alg=aes128-sha1;modp1024\n\tike=aes128-sha1;modp1024\n\tkeyingtries=%forever\n\tkeyexchange=ike\n\tleftsubnet=10.2.0.0/16\n\trightsubnet=10.0.0.0/16\n\tdpddelay=10\n\tdpdtimeout=30\n\tdpdaction=restart_by_peer" > /etc/ipsec.d/aws.conf;

              echo "$EIP $1: PSK \"$2\"" > /etc/ipsec.d/aws.secrets;

              service ipsec restart;
              EOF

              chmod +x connection-setup.sh
              
      NetworkInterfaces:
        - NetworkInterfaceId: !Ref OpenSwanENI
          DeviceIndex: 0
      ImageId: ami-0713f98de93617bb4
      InstanceType: t2.micro
      IamInstanceProfile:
        Ref: InstanceProfileEC2
      Tags:
        - Key: Name
          Value: OpenSwanInstance
