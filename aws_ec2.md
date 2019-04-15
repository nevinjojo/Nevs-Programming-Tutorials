# Launch your website using AWS EC2 Instance

Amazon Elastic Compute Cloud (Amazon EC2) is a web service that provides secure, resizable compute capacity in the cloud. It is designed to make web-scale cloud computing easier for developers.

## Part Uno: Launching an EC2 Instance
I am going to assune that you have created an AWS account, verified your account etc.

Log into the AWS console. Select EC2 and then click on Launch Instance.

### Step 1: Choose an Amazon Machine Image (AMI)
An Amazong Machine Image is the type of OS you will have on your EC2 Virtual Machine.
We are going to choose Amazon Linux 2 AMI 64-bit (x86). Press Select.

### Step 2: Choose an Instance Type
As AWS states "Instances are virtual servers that can run applications. They have varying combinations of CPU, memory, storage, and networking capacity". For our purpose we only need the smallest and most general type called `t2.micro`.

### Step 3: Configure Instance Details
We are going to choose the default settings since that is cheap, simple and fits our needs. Press 'Add Storage'.

### Step 4: Add Storage 
We are again going to choose the default sotrage settings. Note that if you are eligible for 'free tier', you can get upto 30GB General Purpose (SSD).

#### Storage Settings: Explained
EBS Volume is a virtual hard disk in the cloud. Root means it is where we are going to boot our Operating System from. Delete on Termination means the EBS will be deleted if we delete the EC2 instance.

### Step 5: Add Tags
Tags help you to manage and administer your AWS resources (i.e. helps you keep track of cost). We don't really need one. But for the sake of learning, you could create a key called 'Name' and a value called 'WebServer' as suggested by AWS so that you'll know what feature is using up the resouce the most. Press 'Configure Security Group'.

### Step 6: Configure Security Group
This is where you specify the type of traffic your virtual machine will allow from outside. Since you will be accessing the EC2 to install things and develop the website, we will allow SSH. We would also like other people to access the website via HTTP/HTTPS. Therefore we have to allow those types as well. Add SSH, HTTP, and HTTPS types to the security groups with the default protocols, port ranges and sources. Click 'Review and Launch'.

### Step 7: Review Instance Launch
Assuming you did everything right (fingers crossed), you can review the settings and click Launch.

### Step 8: Setting up Private and Public keys for your instance.
A pop up window will ask you to select or create a key pair. A key pair is needed to securely SSH (“log in”) to our new EC2 instance.

* Select ‘Create a new key pair’
* Give the Key Pair a name — e.g. ‘ec2-key-pair’
* Click Download Key Pair
* Click Launch Instances
* Click View Instance to navigate back to the EC2 dashboard.

And there you have it. You have succesfully set up a running instance of an EC2 Virtual Machine. Yay!




