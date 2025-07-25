# Jenkins-installation

 ![jenkins image](https://github.com/Prudhvi5442/Jenkins/blob/f946bc61d96f9cfe8d58f231f18a7d8ba70f8169/images/jenkins.jpeg)

## AWS EC2 Instance

- Go to AWS Console
- Instances(running)
- Launch instances
 
![ec2 image](https://github.com/Prudhvi5442/Jenkins/blob/f946bc61d96f9cfe8d58f231f18a7d8ba70f8169/images/ec2.png)

### Install Jenkins.

Pre-Requisites:
 - Java (JDK)

### Run the below commands to install Java and Jenkins

Install Java

```
sudo apt update
sudo apt install openjdk-17-jre
```

Verify Java is Installed

```
java -version
```

Now, you can proceed with installing Jenkins

```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

**Note: ** By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open port 8080 in the inbound traffic rules as show below.

- EC2 > Instances > Click on <Instance-ID>
- In the bottom tabs -> Click on Security
- Security groups
- Add inbound traffic rules as shown in the image (you can just allow TCP 8080 as well, in my case, I allowed `All traffic`).

### Login to Jenkins using the below URL:

http://<ec2-instance-public-ip-address>:8080    [You can get the ec2-instance-public-ip-address from your AWS EC2 console page]

Note: If you are not interested in allowing `All Traffic` to your EC2 instance
      1. Delete the inbound traffic rule for your instance
      2. Edit the inbound traffic rule to only allow custom TCP port `8080`
  
After you login to Jenkins, 
      - Run the command to copy the Jenkins Admin Password - `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
      - Enter the Administrator password
      
![unlock jenkins image](https://github.com/Prudhvi5442/Jenkins/blob/f946bc61d96f9cfe8d58f231f18a7d8ba70f8169/images/jenkins1.png)

### Click on Install suggested plugins

![customize jenkins image](https://github.com/Prudhvi5442/Jenkins/blob/f946bc61d96f9cfe8d58f231f18a7d8ba70f8169/images/jenkins2.png)

Wait for the Jenkins to Install suggested plugins

Create First Admin User or Skip the step [If you want to use this Jenkins instance for future use-cases as well, better to create admin user]

![creating jenkins user image](https://github.com/Prudhvi5442/Jenkins/blob/f946bc61d96f9cfe8d58f231f18a7d8ba70f8169/images/jenkins3.png)


Jenkins Installation is Successful. You can now starting using the Jenkins 

![jenkins ready image](https://github.com/Prudhvi5442/Jenkins/blob/f946bc61d96f9cfe8d58f231f18a7d8ba70f8169/images/jenkins4.png)
## Install the Docker Pipeline plugin in Jenkins:

   - Log in to Jenkins.
   - Go to Manage Jenkins > Manage Plugins.
   - In the Available tab, search for "Docker Pipeline".
   - Select the plugin and click the Install button.
   - Restart Jenkins after the plugin is installed.
   
![docker pipeline image](https://github.com/Prudhvi5442/Jenkins/blob/f946bc61d96f9cfe8d58f231f18a7d8ba70f8169/images/jenkins5.png)

Wait for the Jenkins to be restarted.


## Docker Slave Configuration

Run the below command to Install Docker

```
sudo apt update
sudo apt install docker.io
```
 
### Grant Jenkins user and Ubuntu user permission to docker deamon.

```
sudo su - 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
```

Once you are done with the above steps, it is better to restart Jenkins.

```
http://<ec2-instance-public-ip>:8080/restart
```

The docker agent configuration is now successful.



