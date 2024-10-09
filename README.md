# Jenkins-CI/CD-Apache2

Steps to follow in Automating a Static Website with Jenkins using Apache2 Web Server

## Create an EC2 with the appropriate instance type. (e.g t2.micro, *t2.medium* was used in this tutorial)

- Give the instance a name
- AMI Type - **Ubuntu**
- Create a key-pair
- Create a **security group** (allow only SSH at this point)
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

- Head over to **Jenkins documentation website** - <https://www.jenkins.io/doc/book/installing/linux/> on Linux installation since we are making use of the Ubuntu OS on our EC2 instance

The first thing is install **Java**

`sudo apt update`
`sudo apt install fontconfig openjdk-17-jre`
`java -version`

- Jenkins for Ubuntu

```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update

sudo apt-get install jenkins
```

`sudo systemctl enable jenkins` to enable jenkins
`sudo systemctl start jenkins` to start jenkins
`sudo systemctl status jenkins` to check jenkins status

### ----- ----- ------ ------ ------ ------- --------- --------

### Connecting to the Jenkins Server

- First, Let us configure our security group to allow this kind of traffic
 Click on the Instance and **select the security tab**, edit the inbound rule by adding the following rules to the initial SSH 22, it will look like the following
 ![Reference Image](/screenshots/inbound%20rule.jpg)

- Copy the public ip address of the EC2 instance and attach it to the port 8080 like this, `public ip:8080` and open this on a new tab to launch your Jenkins page.

- Run `cat /var/lib/jenkins/secrets/initialAdminPassword` access your jenkins Administrator password
- Click on **Install suggested plugins**
- Create **First Admin User** and input the necessary information
- Click on **Save and Continue**
- Click on **Save and Finish**

- Click **New Item**
- Input an **item name**, do not add space in the item name, click **Ok** and enter a Description of what you're trying to build or automate.
- Under Source Code Management, select **Git**
- Copy the repo from Github, http link and post in the Repository URL
- Branch main or master, depends on what you have
- Select **GitHub hook trigger for GITScm poling**
- Click **Save**
- Run the following command `cd /var/www/html`
- Copy the **Console Output** of the **Building in Workspace ... /var/lib/jenkins/workspace/ab-jenkins ...** and past it in the shell e.g `cd /var/lib/jenkins/workspace/ab-jenkins`
- `cd  /var/www`
- Run `usermod -aG www-data jenkins` to add the user "jenkins" to the "www-data" group on a Linux/Unix-based system
- Run `sudo chown jenkins:www-data html` to change the ownership of the html directory (or file) so that the user "jenkins" becomes the owner and the group "www-data" is the group owner
- Run `ls -la` to list the contents of a directory in long format with detailed information, including hidden files

- In jenkins, under the Build Steps, select **Execute shell** and put the following commands
- `rm /var/www/html/index.html` This command remove the default Apache2 web server homepage
- `cp -r * /var/www/html/`  This copy all files and directories from the current working directory to the /var/www/html/

#### Time to plan for CD - Continuous Delivery of our code

- Head over to GitHub and Click on Settings
  
- Click on Webhooks and create a new Webhook
- Payload URL <http://ip:8080/github-webhook/>
- Content type - **application/json**
- Secret- Leave it empty
- Click **Add Webhook**

#### You can go over to edit your code on your PC and Push to GitHub, you will see that your Jenkins will build it automatically with the help of the Webhook

## Thank you for reading  :star_struck:
