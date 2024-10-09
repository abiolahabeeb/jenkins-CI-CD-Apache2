# jenkins-CI-CD-Apache2

Steps to follow in Automating a Static Website with Jenkins using Apache2 Web Server

## Create an EC2 with the appropriate instance type. (e,g t2.micro, **t2.medium** was used in this tutorial)

- Give the instance a name
- AMI Type - **Ubuntu**
- Create a key-pair
- Create a *security group* (allow only SSH at this point)
- Launch an instance

## SSH into the EC2 instance via the SSH Client

- Run `who` to who currently SSH into the instance

- Run `sudo su -` to enter the root folder of the instance

- Run `apt update` to update packages on the instance
- Run `sudo apt install apache2` to install Apache2 web server
- Verify Apache Installation `which apache2` or `apache2 -v`
- `sudo service apache2 start`
- `sudo service apache2 status`

### Copy the instance public ip to another tab and view the Apache homepage before proceeding with other steps

## Jenkins Installation

- Head over to *Jenkins documentation website* - <https://www.jenkins.io/doc/book/installing/linux/> on Linux installation since we are making use of the Ubuntu OS on our EC2 instance

The first thing is install **Java**

`sudo apt update`
`sudo apt install fontconfig openjdk-17-jre`
`java -version`

- Jenkins for Ubuntu
  `sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \`
  `https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key`
space
`echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \`
  `https://pkg.jenkins.io/debian-stable binary/ | sudo tee \`
  `/etc/apt/sources.list.d/jenkins.list > /dev/null`
space
`sudo apt-get update`
`sudo apt-get install jenkins`
  