### Project Overview
This project was about configuring a Linux Web Server from the ground up. An EC2 instance with a baseline installation of Linux was used for this project. Securing the server from a number of attack vectors, installing and configuring a database server, and deploying an existing web application onto it, were part of this project. The goal was to have a web application running live on a secure web server. The URL to the hosted web application is: http://ec2-3-121-184-12.eu-central-1.compute.amazonaws.com .

### Setting up and SSH into the Amazon EC2 Instance
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
3.1 Open the configuration file with the following command: **sudo nano /etc/ssh/sshd_config**
3.2 Locate port 22 in the file and replace it with 2200. Than save the changes.
3.3 Restart SSH with the following command: **sudo service ssh restart**
4.  Configure the EC2 instance's firewall to allow inbound rules on port 2200.
 
### Configure the Uncomplicated Firewall (UFW)
1.TODO