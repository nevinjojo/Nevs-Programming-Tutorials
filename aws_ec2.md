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



## Part Dos: SSH into EC2
### Change permission of key-value-pair file and store it safely.
Open Terminal. Find the file ec2-key-value file and change the permissions.
`chmod 400 path/to/key/value/pair/file`

Move the ec2-key-value file you downloaded to the ~/.ssh/ directory.
`mv path/to/key/value/pair/file ~/.ssh/ec2.pem`

### SSH into your EC2 Istance
On the AWS EC2 dashboard, you will see the new instance of your VM. Selecting the instance (click the button next to the instance) displays information about the instance below. In this area, you will see the IPv4 Public IP address of your instance. Copy it to your clipboard.

On the terminal run this command to ssh into the ec2 instance:
`ssh -i ~/.ssh/ec2.pem ec2-user@<public-ip_addr_from_aws_dashboard>`

The prompt will ask you if you want to permanently add the IP address to the list of known hosts. Type `yes` and press enter.

### Bonus: Create alias for your ec2 instance on .ssh/config
Open the SSH config file by running `vim ~/.ssh/config`.

Add the following command to the config, save the file and exit out of the file.
```
Host ec2
    Hostname <public_ip_addr_from_aws_dashboard>
    Port 22
    User <your_username>
    IdentityFile ~/.ssh/ec2.pem
```

Now run `ssh ec2` to log in to the ec2 instance. Handy Dandy!

### Elevate your privileges and Update packages on the VM
```
sudo su
yum update -y
```

## Part Tres: Host a static webpage on EC2
### Install Apache Webserver
`yum install httpd -y`

### Start the Webserver
`service httpd start`

Configure the web server to restart of it randomnly stops
`chkconfig httpd on`

### Install git on EC2
`yim install git -y`

### Option I: Clone a repo from Github
#### Generate SSH keys on EC2 and add it to Github account
`ssh-keygen -t rsa -C "your-email@gmail.com"`

Copy the public key from `id_rsa.pub` and store it to the list of SSH keys on your Github account. You can learn more about how to configure github account to your new SSH key [here](https://help.github.com/en/articles/adding-a-new-ssh-key-to-your-github-account). 

Once the basic setup is complete, clone your project from github to `/var/www/html`
`git clone git@github.com:<username>/<repo_name>.git`

N.B.1: You might get a permission denied prompt if you tried to clone to `/var/www/html` while not being a root user. Therefore you need to `sudo su` before cloning.
N.B.2: Make sure that the index.html file of your website is visible from `/var/www/html` after you clone the repo. You might have to move all of the contents of the repository folder to `/var/www/html` in order for the normal running of the site.

### Option II: Create a static site from scratch
Navigate to the directory `cd /var/www/html`.

Create an `index.html` file
`vim index.html`

Add contents to the HTML file
```
<html>
<body>
    <h1>LOL I'm running this very average website from an EC2 Instance.</h1>
</body>
</html>
```
Save and exit out the file.


### View your static website
Navigate back to the EC2 dashboard in the AWS console and copy the Public DNS(IPV4) of your instance into your clipboard. Paste that address into your browser.

Tada! You just launched a website using an AWS EC2 Instance!
