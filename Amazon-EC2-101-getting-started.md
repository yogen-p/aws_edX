## Amazon EC2 101 getting started

Here, you will create your first Amazon EC2 instance and install a sample Python Flask app using user data. When you launch an instance in Amazon EC2, you can pass user data to the instance that can be used to perform common automated configuration tasks. User data is usually passed in the form of shell-scripts. You can pass this data into the launch wizard as plain text, as a file while launching via the command line tools, or as base64-encoded text for API calls.

To get started, follow the steps below.
### Launch an Amazon EC2 instance with a user data script.

In this section, you will launch an Amazon EC2 instance with a user data script. If you are familiar with Amazon EC2, you may want to attempt to complete this section by using the properties below before reading the step-by-step instructions.

Region: Oregon (us-west-2)

Amazon Machine Image (AMI): Amazon Linux AMI (Do not use the Amazon Linux 2 AMI)

Instance Type: t2.micro

User data script: 

```
#!/bin/bash -ex
sudo yum update -y
sudo pip install flask
sudo pip install requests
mkdir PythonWebApp
cd PythonWebApp
sudo cat >> flaskApp.py << EOF
from flask import Flask
import requests
app = Flask(__name__)
@app.route("/")
def main():
  r = requests.get('http://169.254.169.254/latest/dynamic/instance-identity/document')
  text = "Welcome! Here is some info about me!\n\n" + r.text
  return text


if __name__ == "__main__":
  app.run(host='0.0.0.0', port=80)
EOF
sudo python flaskApp.py

```

Tag: SamplePythonFlaskApp

Security group name: exercise2-sg

Security group rules: Allow HTTP

Key Pair: Proceed without a key pair


### Step-by-step instructions to launch an Amazon EC2 instance

-    In the AWS Console, click Services, then click EC2 to open the EC2 dashboard.
-    At the top-right corner, select the US West (Oregon) region.
-    From the EC2 dashboard, click Launch Instance.
-    On the Choose an Amazon Machine Image (AMI) page, select Amazon Linux AMI by clicking Select. This AMI is free-tier eligible.
-    Note: Do not select the Amazon Linux 2 AMI option.
-    On the Choose an Instance Type page, you can select the hardware configuration of your instance. Select t2.micro.
-    Click Next: Configure Instance Details.
-    On the Configure Instance Details page, leave the defaults and scroll down to the Advanced Details section and expand it.
-    In the User data section, leave As text selected.
-    Copy and paste the contents of the user data script in the text area.
-    Click Next: Add Storage. Skip through this page and click Next: Add Tags.
-    Click Add Tag. Tags enable you to categorize your AWS resources in different ways - for example, by purpose, owner, or environment.
-    In the Key textbox, type Name
-    In the Value textbox, type SamplePythonFlaskApp
-    Click Next: Configure Security Group. Note that the wizard gives you an option to create a new security group or select an existing one. For this exercise, accept the default chosen option, Create a new security group.
-    For Security Group Name, type exercise2-sg
-    In the security group table, delete the SSH rule by clicking the X button at the end of the row.
-    Click Add Rule.
-    For Type, leave Custom TCP Rule selected.
-    For Port Range, type 80
-    For Source, type 0.0.0.0/0
-    Click Review and Launch.
-    On the Review Instance Launch page, review the details and click Launch.
-    When prompted for a key pair, select Proceed without a key pair.
-    Select the acknowledgement check box, and then click Launch Instances.
-    Click View Instances to return to the Instances page.
-    On the Instances page, you can view the status of the launch. It can take a few minutes for the instance to be ready so that you can connect to it. Check that your instance has passed its status checks. You can view this information in the Status Checks column.
-    Note: It takes a few minutes for the status checks to pass. Wait until the status checks changes from Initializing to 2/2 checks passed.
-    Once the instance is ready, select the instance and write down the IPv4 Public IP found in the Descriptions tab at the bottom.

### Test the sample app running on your instance

-    Open a browser and type the public IP of the Amazon EC2 instance you copied earlier.
-    You should see a sample Python app running on your Amazon EC2 instance.
-    Congratulations! You have launched your first web server in AWS.

### Terminate the Amazon EC2 instance

In this section, you will terminate the Amazon EC2 instance by selecting the instance in EC2 dashboard and clicking Actions -> Instance State -> Terminate .

### Step-by-step instructions to terminate the Amazon EC2 instance

-    In the AWS Console, click Services, then click EC2 to open the EC2 dashboard.
-    In the navigation pane, click Instances. In the list of instances, select the SamplePythonFlaskApp instance.
-    Click Actions, Instance State, Terminate.
-    Click Yes, Terminate when prompted for confirmation.
-    Amazon EC2 shuts down and terminates your instance. After your instance is terminated, it remains visible on the console for a short while, and then the entry is deleted.

