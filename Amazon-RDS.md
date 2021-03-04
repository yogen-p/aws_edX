## Amazon RDS database

Here we create an Amazon RDS database instance and store the Amazon S3 object key for the photo and photo labels in the database. This way, you are storing your data in a more structured format and making the application scalable.
Note: Make sure to sign in to your AWS account with the AWS IAM user edXProjectUser credentials.

To get started, follow the instructions below.
1. Create an Amazon RDS database instance.

In this section, you will create an Amazon RDS instance with the properties shown below to store photos and labels. If you are familiar with Amazon RDS, you may want to attempt to complete this section before reading the step-by-step instructions.

Region: Oregon (us-west-2)
Amazon RDS Instance type: MySQL (free tier eligible)
Name of DB instance: edx-photos-db
Master username: master
Master user password: Type a master user password and write it down for later use.
VPC: edx-build-aws-vpc
Database name: Photos
Important: Make a note of the database endpoint.

### Step-by-step instructions for creating Amazon RDS database on MySQL.

-    In the AWS Console, click Services, then click Relational Database Service to open the Amazon RDS dashboard.
-    Make sure you are still in the Oregon AWS Region.
-    On the left navigation menu, click Instances.
-    Click Launch DB instance.
-    Scroll down to the bottom and select the Only enable options eligible for RDS free usage tier option.
-    Scroll back to the top and select MySQL.
-    Click Next.
-    Leave the default selections and scroll down to Settings.
-    For DB instance identifier, type edx-photos-db
-    For Master username, type master
-    Type a password for the master user and confirm the password. Make a note of the password for later use.
-    Click Next.
-    In the Network & Security section, select edx-build-aws-vpc.
-    Scroll down to Database options.
-    For Database name, type Photos. Make a note of the database name for later use.
-    Leave the rest of the default settings, scroll down to the bottom and click Launch DB instance.

-    Note: It should take about five minutes for the instance to launch.
-    Click View DB instance details to go to the DB instance details page.
-    After the instance launches, scroll down to the Connect section and make a note of the Endpoint for later use.
-    The endpoint will look like this: sample.cppyk3cpwnox.us-west-2.rds.amazonaws.com.

2. Modify the security group of the Amazon RDS database.

In this section, you will modify the security group of the Amazon RDS instance to the security group of the AWS Cloud9 instance.

-    On the Amazon RDS database instance page, scroll down to the Details section.
-    Under Security and network, click the security group. The security group should have a name like rds-launch-wizard-xxx. A new page displaying the security group you just clicked should open.
-    Make a note of the security group ID. You will need it in subsequent exercises.
-    On the bottom pane, click Inbound.
-    Click Edit.
-    In the Source textbox, delete the existing text and type sg. A list of security groups will appear. Select the security group that contains your AWS Cloud9 environment name.
-    Click Save.

3. Download the exercise code .zip file and unzip it to your AWS Cloud9 environment.

In this section, you will download the exercise code .zip file and unzip it to your AWS Cloud9 environment. If you feel familiar with the AWS Cloud9 environment from the previous few exercises, you may want to attempt to complete this section before reading the step-by-step instructions.

```
wget https://s3-us-west-2.amazonaws.com/us-west-2-tcdev/courses/AWS-100-ADG/v1.1.0/exercises/ex-rds.zip
unzip ex-rds.zip
```

4. Explore the exercise code.

-    In your AWS Cloud9 environment, on the left tree view, notice the exercise-rds/SetupScripts/database_create_tables.py file. This script creates the database tables needed for the application. The photo table stores the photos and labels information. The web_user is a restricted privilege user who has access solely to the photos table. The web application is configured to use the web_user and not the master user.
-    Open the exercise-rds/FlaskApp/database.py file and explore the code for adding a photo to the database and fetching it back from the database.

5. Run the database script.

-    To run the database_create_tables.py script, type the command below in your AWS Cloud9 terminal window.

-    python3 exercise-rds/SetupScripts/database_create_tables.py
-    You should see a prompt on the screen to configure the database information. Follow the prompts and enter the information as shown below.
-    Database host: Paste the database endpoint you noted earlier.
-    Database user: master
-    Database password: Type the password for the master user.
-    Database name: Photos
-    web_user password: Type a password for the web_user. Make a note of the web_user password for later use.

-    You should see a message that a web_user is created with the required access granted to it.

6. Configure environment variables and run and test the code.

-    For the code to run successfully, you will need to configure the environment variables with the database details. Under the FlaskApp folder, open the config.py file. Notice that the config.py file is now updated with the database-related environment variables.
-    In your AWS Cloud9 environment, on the Run Configuration pane at the bottom, click ENV on the right side. You should see a small list showing the previously configured environment variables.
-    To configure the database environment variables, type the environment variable Name and Value as shown in the table below:
---------------------
| Name | 	Value |
---------------------
| DATABASE_HOST | 	Database endpoint you noted earlier |
| DATABASE_USER | 	web_user |
| DATABASE_PASSWORD | 	Password for the web_user you noted earlier |
| DATABASE_DB_NAME | 	Photos |
---------------------------------
-    Note: Make sure to delete any white space inserted while copy/pasting.
-    To run the code, you will need to point the Run Configuration to the correct exercise folder. On Python3RunConfiguration pane at the bottom, in the Command textbox, type the text shown below and click Run on the left side.

-    exercise-rds/FlaskApp/application.py

-    You should see a message like this one:

        Running on http://0.0.0.0:8080

-    To test the application, click Preview -> Preview Running Application on the top menu bar of the Cloud9 environment.
-    Pop out the application in a new window by clicking the Pop Out button.
-    Click Home and upload a photo. You should see the photo and the Amazon Rekognition labels generated for the photo.
-    Click Home. You should see a table with the thumbnail for the photo and the label information. This information is being fetched from the database.
-    Try uploading a few more photos and watch as the table on the Home page is populated with the information saved in the database.

Optional Challenge

There is a command line mysql client on your Cloud9 instance.

The mysql client takes parameters for the database host and user. A "-p" switch tells the client to prompt you for a password. To connect to your RDS database, run the command below (replace DATABASE_HOST with your RDS database endpoint).
mysql -h DATABASE_HOST -u web_user -p

Can you SELECT the contents of the photo table?

7. Stop the Amazon RDS database instance.

To keep your AWS account bill to a minimum, consider stopping the Amazon RDS instance and then starting it again when needed. Follow the steps below to stop the Amazon RDS database instance.

-    In the AWS Console, click Services, then click Relational Database Service to open the Amazon RDS dashboard.
-    In the left navigation pane, click Instances. From the list of instances, select edx-photos-db.
-    At the top, click Instance actions -> Stop. You will get a prompt. Click Yes, stop now.

