# AWS-Project-One

+================================================================================+


Send Fanout Event Notifications with Amazon Simple Queue Service (SQS) and Amazon Simple Notification Service (SNS)

+================================================================================+
Performed By - Sonam Gupta



We will explore how to utilize AWS Systems Manager for executing remote commands on your Amazon EC2 instances. Systems Manager serves as a management tool that empowers us to gain operational insights and securely take action on AWS resources at scale. By leveraging the run command feature of Systems Manager, we can simplify the management tasks by eliminating the need for bastion hosts, SSH, or remote PowerShell.

In our example scenario, as a System Administrator, our objective is to update the packages on your EC2 instances. However, this task is complicated by the fact that our security team restricts direct access to production servers via SSH and prohibits the use of bastion hosts. Fortunately, we can rely on Systems Manager to remotely execute commands, such as package updates, on your EC2 instances.

To overcome this challenge, we will implement the following steps:

1.Create an Identity and Access Management (IAM) role.
2.Enable an agent on your EC2 instance that establishes communication with Systems Manager.
3.Follow best practices by running the AWS-UpdateSSMAgent document to upgrade your Systems Manager Agent.
4.Utilize Systems Manager to execute a command on your EC2 instance.

By following these steps, we will be able to effectively manage and administer your EC2 instances using Systems Manager while adhering to recommended practices.

+================================================================================+
CREATE AN IDENTITY AND ACCESS MANAGEMENT (IAM) ROLE
+================================================================================+

During this phase, you will generate an IAM role intended for granting Systems Manager the necessary authorization to execute actions on your instances.

Open the IAM console at https://console.aws.amazon.com/iam/.

<img width="361" alt="image" src="https://github.com/sonam-gupta-letsconnect/AWS-Project-One/assets/140079682/4f4d45d7-aaec-4e41-9677-789297afdb17">


But before we proceed further, Let’s solve the security recommendation that I faced during the execution. It was a hindrance to utilizing IAM resources. Because MFA for root users is not registered. So let’s add.







Keep following the steps, to successfully complete this task, you will need a Visa/MasterCard credit or debit card.
AWS will deduct INR 2 for verification.
Also, you will be asked to download the Google Authenticator for future logins.










After following all the steps you will be redirected to this page where you can see the message: MFA Device Assigned














You are now supposed to re-login to your aws account as a root	user	at https://aws.amazon.com/






Going back to the previous window, you can see everything is now in green and we are good to go.
















In the left navigation pane, choose Roles, and then choose Create role or you can directly choose from the dropdown as shown.












On the Select trusted Entity page, under AWS Service, choose EC2, and then choose Next.





On the Add permissions page, in the search bar type AmazonEC2RoleforSSM.
From the policy list select AmazonEC2RoleforSSM and then choose Next.











On the Name, review, and Create page, enter "EnablesEC2ToAccessSystemsManagerRole" in the Role name field.
Provide a brief description in the Description box, such as "Enables an EC2 instance to access Systems Manager."








Choose Create role.



+================================================================================+
CREATE AN EC2 INSTANCE
+================================================================================+


In this stage, you will generate an EC2 instance utilizing the role named EnablesEC2ToAccessSystemsManagerRole. This will grant the EC2 instance the ability to be controlled and managed by Systems Manager.







Open the Amazon EC2 console. From the EC2 console, select your preferred Region. Systems Manager is supported in all AWS Regions. Now choose Launch instance.






Fill in the Name field with "MyEC2Tutorial". Choose the Amazon Linux AMI from the available options, keeping the default selection in the dropdown.

Additionally, if desired, you have the option to install the Systems Manager Agent on your own Windows or Linux system.







Choose the t2.micro instance type.













You will not need a keypair to use Systems Manager to remotely run commands. Scroll down to Key pair and under the Key pair name dropdown, choose Proceed without a key pair.










Under Advanced details, in the IAM instance profile dropdown choose the EnablesEC2ToAccessSystemsManagerRole role you created earlier. Leave everything else as default.









Choose Launch instance.























Success!

+================================================================================+
UPDATE THE SYSTEMS MANAGER AGENT
+================================================================================+


With an EC2 instance now actively running the Systems Manager agent, we gain the ability to automate administrative tasks and effectively manage the instance. In this particular phase, we will execute a pre-packaged command, referred to as a document, designed to upgrade the agent. It is considered a best practice to update the Systems Manager Agent whenever a new instance is created.






In the top navigation bar, search for Systems Manager and open the Systems Manager console.











Under the Node Management section on the left navigation bar, choose Fleet Manager.
















Selecting the node ID created in step 2, MyEC2Tutorial, to open the node detail page.
















On the node detail page, in the Node actions dropdown, select Execute run command.











On the Run a command page, click in the search bar and select Document name prefix, then click on Equals, then type in AWS-UpdateSSMAgent.

Now select the radio button on the	left	of AWS-UpdateSSMAgent. This document will upgrade the Systems Management agent on the instance.












Scroll down to the Targets panel and select the check box next to your managed EC2 instance.

Finally, scroll down and select Run.











Next, you will see a page documenting your running command, and then overall success in green. Congrats, you have just run your first remote command using Systems Manager.
















Success!

+================================================================================+
RUN A REMOTE SHELL SCRIPT
+================================================================================+


Now that your EC2 instance has the latest Systems Manager Agent, you can upgrade the packages on the EC2 instance. In this step, you will run a shell script through Run Command.





Under the Node Management section on the left navigation bar, choose Fleet Manager.













Select the node ID created in step 2, MyEC2Tutorial, to open the node detail page.












On the Run a command page, click in the search bar and select Document name prefix, then click on Equals, then type in AWS-RunShellScript.







Now select the radio button on the left of AWS-RunShellScript.
















Scroll down to the Command Parameters panel and insert the following command in the Commands text box:

sudo yum update –y














Scroll down to the Targets panel and select the check box next to your managed EC2 instance.

Finally, scroll down and select Run.












As your script executes remotely on the managed EC2 instance, the Overall status will initially show as "In Progress." However, it will transition to "Success" shortly thereafter. Once it reaches the "Success" status, scroll down to the Targets and Outputs panel. From there, locate and choose the Instance ID of your specific instance. Please note that your Instance ID will differ from the one displayed in the illustration.






From the Output on: i-XX page, select the header of the Output panel to view the output of the update command from the instance.











+================================================================================+
TERMINATE YOUR RESOURCES
+================================================================================+
We will terminate your Systems Manager and EC2-related resources. Important: Terminating resources that are not actively being used reduces costs and is a best practice.





Open the Amazon EC2 console and from the left navigation under the Instances heading, select Instances.












Select your instance's checkbox and choose Instance state, then select Terminate instance. This will terminate your instance completely.





















All resources used to perform this project are now terminated and are free.

















– The End –
