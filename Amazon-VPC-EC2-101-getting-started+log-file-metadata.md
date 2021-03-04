## AWS CloudFormation template to create VPC and subnets.

In this section, you will create a VPC and subnets by launching an AWS CloudFormation template. If you are familiar with AWS CloudFormation, you may want to attempt to complete this section by using the properties below before reading the step-by-step instructions.

Region: Oregon (us-west-2)

CloudFormation template: Download template

Name of the stack: edx-vpc-stack


### Step-by-step instructions for AWS CloudFormation.

-    In the AWS Console, click Services, then click CloudFormation to open the CloudFormation dashboard.
-    Make sure you are still in the Oregon AWS Region.
-    Click Create Stack.
-    Download the AWS CloudFormation template to create a VPC and save it locally on your computer.
-    To select the AWS CloudFormation template you just downloaded, click Choose file.
-    Click Next.
-    In the Stack name textbox, type edx-vpc-stack.
-    Click Next. Skip the Options page and click Next.
-    Click Create. You will notice that the status of the template is CREATE_IN_PROGRESS. The template should finish creating in a minute.
-    In your AWS Management Console home page, in the AWS services search textbox at the top, type VPC, and then select VPC to open the VPC dashboard.
-    In the VPC dashboard, on the left navigation menu, click Your VPCs. You will see a VPC named edx-build-aws-vpc in the list. Write down the vpc-id of the edx-build-aws-vpc. You will need the vpc-id in subsequent exercises.
-   In the VPC dashboard, on the left navigation menu, click Subnets. You will see four subnets starting with edx-subnet-xxxx. Write down the subnet-id of edx-subnet-public-a. You will need the subnet-id in subsequent exercises.

### Launch an Amazon EC2 instance with a user data script in a VPC.

In this section, you will launch an Amazon EC2 instance with an user data script. If you are familiar with Amazon EC2, you may want to attempt to complete this section by using the properties below before reading the step-by-step instructions.

Region: Oregon (us-west-2)

Amazon Machine Image (AMI): Amazon Linux AMI (Do not use the Amazon Linux 2 AMI)

Instance Type: t2.micro

Network VPC: edx-build-aws-vpc

Subnet: edx-subnet-public-a

User data script: Download

Tag: Ex3WebServer


Security group name: exercise3-sg

Security group rules: Allow HTTP and SSH

Key Pair: Create a new key pair and save it for later use.


### Step-by-step instructions Amazon EC2 instance.

-    In the AWS Console, click Services, then click EC2 to open the EC2 dashboard.
-    At the top right corner, select the US West (Oregon) region.
-    From the EC2 dashboard, click Launch Instance.
-    On the Choose an Amazon Machine Image (AMI) page, select Amazon Linux AMI by clicking Select. This AMI is free-tier eligible.    Note: Do not select the Amazon Linux 2 AMI option.
-    On the Choose an Instance Type page, select t2.micro.
-    Click Next: Configure Instance Details.
-    For Network, select edx-build-aws-vpc.
-    For Subnet, select edx-subnet-public-a.
-    Leave the defaults and scroll down to the Advanced Details section and expand it.
-    Download the user data script and copy and paste the contents of the script in the text area.
-    Click Next: Add Storage. Skip through this page and click Next: Add Tags.
-    Click Add Tag.
-    In the Key textbox, type Name
-    In the Value textbox, type Ex3WebServer
-    Click Next: Configure Security Group. Note that the wizard gives you an option to create a new security group or select an existing one. For this exercise, leave the default chosen option, Create a new security group.
-    For Security group name, type exercise3-sg
-    Click Add Rule.
-    For Type, leave Custom TCP Rule selected.
-    For Port Range, type 80
-    For Source, type 0.0.0.0/0
-    Note: The inbound rule for SSH is added by default.
-    Click Review and Launch.
-    On the Review Instance Launch page, review the details and click Launch.
-    When prompted for a key pair, select Create a new key pair, enter a name for the key pair, and then click Download Key Pair.
-    Note: This is the only chance for you to save the private key file, so be sure to download it. You will use the same key pair for all subsequent exercises in the course. Save the private key file in a safe place. You'll need to provide the name of your key pair when you launch an instance and the corresponding private key each time you connect to the instance.
-    Select the acknowledgement check box, and then click Launch Instances.
-    Click View Instances to return to the instances page.
-    On the Instances page, you can view the status of the launch. It can take a few minutes for the instance to be ready so that you can connect to it. Check that your instance has passed its status checks. You can view this information in the Status Checks column.
-    Note: It takes a few minutes for the status checks to pass. Wait until the status checks changes from Initializing to 2/2 checks passed.
-    Once the instance is ready, select the instance and note down the IPv4 Public IP found in the Descriptions tab at the bottom.
-    Paste the public IP address of your instance in your web browser to display the welcome web page. This page is now displaying dynamic information about your server.

### Connect to your Amazon EC2 instance.

In this section, you will connect to your Amazon EC2 instance via SSH. An SSH connection requires port 22 to be open on your network. You may need to contact your network administrator to ensure that this is open.

For MAC/Linux users:

-    Open the Terminal application.
-    Type the commands below. In both commands, replace PATH-TO-PEM-FILE with a reference to the .pem file that you downloaded while launching the instance. In the second command, replace PUBLIC-IP with the IPv4 Public IP of the instance.
```
    chmod 400 PATH-TO-PEM-FILE
    ssh -i PATH-TO-PEM-FILE ec2-user@PUBLIC-IP
```   
-    You will see a prompt like the one below. Answer yes to the prompt.

-    The authenticity of host '54.201.7.240 (54.201.7.240)' can't be established. ECDSA key fingerprint is SHA256:TrCPkFBL0F+pTp3LH+UGFPhGjl7N4qaoLucu21RWsRM. Are you sure you want to continue connecting (yes/no)?



### View log file, query instance metadata, and user data.

In this section, you will inspect the cloud-init logs to verify the steps in the UserData script executed on the Amazon EC2 Instance. Then, you will query the instance metadata service from the Amazon EC2 instance so that you can see how your application is printing information about itself on the welcome page.

-    To view the log file, type the command below in your instance terminal.
```
   cat /var/log/cloud-init-output.log
```
	
-    Explore the log file to see the log entries generated for installing the user data script.
    - To view the instance metadata, type the command below:
```
    curl http://169.254.169.254/latest/meta-data/
```
    	
    - Execute the command below to get the instance identity document of your instance:
```
    curl http://169.254.169.254/latest/dynamic/instance-identity/document
```    
    
    - Execute the command below to get the instance public IP address:
```
    curl http://169.254.169.254/latest/meta-data/public-ipv4
```   
    	
    - Execute the command below to get the MAC address of the instance:
```
    curl http://169.254.169.254/latest/meta-data/mac
```
    	
    - Execute the command below to get the VPC ID in which the instance resides. Make sure to replace Your-MAC in the command below with the MAC address of your instance:
```
    curl http://169.254.169.254/latest/meta-data/network/interfaces/macs/Your-MAC/vpc-id
```
    
    - Execute the command below to get the subnet-id in which the instance resides. Make sure to replace Your-MAC in the command below with the MAC address of your instance:
```
    curl http://169.254.169.254/latest/meta-data/network/interfaces/macs/Your-MAC/subnet-id
```
    
    - Execute the command below to get the instance user data:
```
    curl http://169.254.169.254/latest/user-data
```
    Go to the EC2 dashboard in your console, locate the Amazon EC2 instance you created and verify the public IP address, the VPC ID and the subnet-id of the instance you just queried in the instance terminal. You should be able see this information in the Description tab at the bottom.

### Terminate the Amazon EC2 instance.

In this section, you will terminate the Amazon EC2 instance by selecting the instance in the EC2 dashboard and clicking Actions -> Instance State -> Terminate .

Step-by-step instructions to Terminate the Amazon EC2 instance.

-    In the AWS Console, click Services, then click EC2 to open the EC2 dashboard.
-    In the navigation pane, click Instances. In the list of instances, select the Ex3WebServer instance.
-    Click Actions, Instance State, Terminate.
-    Click Yes, Terminate when prompted for confirmation.
-    Amazon EC2 shuts down and terminates your instance. After your instance is terminated, it remains visible on the console for a short while, and then the entry is deleted.

#### Key Terms:
- Availability Zone - A physically distinct, independent infrastructure, that is engineered to be highly reliable 
- VPC - Virual Private Cloud. Your network in AWS is provided by Amazon Virtual Private Cloud (VPC). You can create a VPC within an AWS region and within that VPC you define subnets to manage related sets of servers or other AWS resources. VPC lets you define rules for how network traffic from your subnets is routed. You can also decide whether your network should be connected to the Internet, to corporate networks, or to keep the network completely private.

The IP Address ranges for VPC and Subnets are specified using CIDR notation. If you'd like to know more about IP addressing within VPC, see the VPCs and Subnets in the User Guide for Amazon VPC. You can also learn more about CIDR notation in section 3.1 of RFC4632 or in Classless Interdomain Routing on Wikipedia.
