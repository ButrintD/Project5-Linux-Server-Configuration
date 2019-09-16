### Project Overview
This project was about configuring a Linux Web Server from the ground up. An EC2 instance with a baseline installation of Linux was used for this project. Securing the server from a number of attack vectors, installing and configuring a database server, and deploying an existing web application onto it, were part of this project. The goal was to have a web application running live on a secure web server. The URL to the hosted web application is: http://ec2-3-121-184-12.eu-central-1.compute.amazonaws.com .

### Setting up and logging into the Amazon EC2 Instance
1. Login to AWS
2. Search for EC2
3. Create a new EC2 instance with an Ubuntu Linux Server
4. Download the private key and paste it to your local machine, in the **.ssh directory**.
5. Change the file permissions with: **chmod 600 "file-name"**
6. SSH to the EC2 instance: **ssh -i ~/.ssh/"file-name" ubuntu@ec2-3-121-184-12.eu-central-1.compute.amazonaws.com**

### Secure the server
1.  Update all currently installed packages: sudo apt-get update
2.  Upgrade packages that need to be updgraded: sudo apt-get upgrade
3.  Change the SSH port from 22 to 2200.
4. Open the configuration file with the following command: **sudo nano /etc/ssh/sshd_config**
5. Locate port 22 in the file and replace it with 2200. Than save the changes.
6. Restart SSH with the following command: **sudo service ssh restart**
7. Configure the EC2 instance's firewall to allow inbound rules on port 2200.
 
### Configure the Uncomplicated Firewall (UFW)
1. Deny all incomings by default: **sudo ufw default deny incoming**
2. Allow all outgoings by default: **sudo ufw default allow outgoing**
3. Allow incoming SSH connections on port 2200: **sudo ufw allow 2200/tcp**
4. Allow incoming HTTP connections on port 80: **sudo ufw allow www**
5. Allow NTP on port 123: **sudo ufw allow 123/udp**
6. Close port 22 for SSH connections: **sudo ufw deny 22**
7. Enable the firewall with the new configuration: **sudo ufw enable**
8. Update the EC2 instance's inbound rules accordingly in the AWS Console (Security Groups) to allow connections to ports 2200, 80, and 123.
9. SSH to the EC2 instance by specifying the port: **ssh -i ~/.ssh/“file-name” ubuntu@ec2-3-121-184-12.eu-central-1.compute.amazonaws.com -p 2200**

### Create a user called **grader** and provide access to the EC2 instance
1. Add the grader user: **sudo adduser grader**
2. Create a file in the sudoers.d directory for the grader user: **sudo touch /etc/sudoers.d/grader**
3. Open the file: **sudo nano /etc/sudoers.d/grader**
3. Add the following content to this file: **grader ALL=(ALL) NOPASSWD:ALL**
4. Save the file

### Setup SSH keys for the grader user
1. Generate a private and public key pair for grader by opening a terminal on your local machine and writting the following: **ssh-keygen**
2. Save it to the ~/.ssh directory
2. Create the ssh directory for grader: **mkdir /home/grader/.ssh**
3. Change the owner to grader: **chown grader:grader /home/grader/.ssh**
4. Change the permissions: **chmod 700 /home/grader/.ssh**
5. Create the authorized_keys file: **sudo nano /home/grader/.ssh/authorized_keys**
6. Copy the content of the generated public key to the **authorized_keys** and save the file.
7. Restart the SSH service: **sudo service ssh restart**
8. Login as grader: **ssh -i "graders private key" grader@ec2-3-121-184-12.eu-central-1.compute.amazonaws.com -p 2200**
9. Disable the password authentication: **sudo nano /etc/ssh/sshd_config** . Search for **PasswordAuthentication** and set it to **no**.

### Change timezone to UTC
1. Excecute the following command: **sudo dpkg-reconfigure tzdata**
2. Select None, this sets the timezone to UTC.

###TODO