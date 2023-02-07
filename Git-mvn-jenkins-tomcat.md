### Downloading and installing Jenkins on LINUX

```
Key applications that need to be installed before installing jenkins
Git
Java
Maven
```
### Step 1 Configuring maven with environment variables
N/B for maven,git,and java configurations if you have performed the previous labs then you dont need to install this applications you can proceed to install jenkins

### Git Installation
//verify if git is installed
```
$ which git
$ Sudo yum install git
$ git  -version
```
### Step 2
### Java in installation
Note the section below for those who ran set the java home path as shown below, To Update your java home path
```
$ sudo yum install java-11-openjdk -y
$ sudo -i
$ echo $JAVA_HOME
//to edit the updated java
$ echo "JAVA_HOME=\"/usr/lib/jvm/java-11-openjdk-11.0.17.0.8-2.el8_6.x86_64 \"" >> /etc/profile
$ echo "PATH=\$JAVA_HOME/bin:\$PATH" >> /etc/profile
$ source /etc/profile
```
### Step 3
### Maven Installation
```
$ which wget
//install wget if not found/installed
$ sudo yum install wget -y
$ cd /tmp/
$ sudo wget https://dlcdn.apache.org/maven/maven-3/3.8.7/binaries/apache-maven-3.8.7-bin.tar.gz
$ cd /tmp ; sudo tar -xzf apache-maven-3.8.7-bin.tar.gz -C  /opt/
$ sudo -i
$ sudo mv /opt/apache-maven-3.8.7 /opt/maven
$ sudo echo "MAVEN_HOME=\"/opt/maven\"" >> /etc/profile
$ sudo echo "PATH=\$MAVEN_HOME/bin:\$PATH" >> /etc/profile
$ source /etc/profile
```
### Step 4
NB Install tomcat by default uses port 8080 so we will need to change its defauult port to maybe 8081 as jenkins also uses port 8080 so they wont be any class. Perform this run the bash script below and restart tomcat. Then proceed to install jenkins.
```
$ vi changeport.sh
```
//paste the commands below in the bash script
```
#! /bin/bash

sed -i 's/8080/8081/g'  /opt/apache-tomcat-9.0.70/conf/server.xml
```
//save the file
```
$ bash changeport.sh
$ cd /opt/apache-tomcat-9.0.70/bin
//in the bin directory run the commands below
$./startup.sh
$ ./shutdown.sh
$./startup.sh
```
### Step 5
### Jenkins installation
```
$ sudo yum update -y

Add the Jenkins repo using the following command:

$ sudo wget -O /etc/yum.repos.d/jenkins.repo \
https://pkg.jenkins.io/redhat-stable/jenkins.repo

##Import a key file from Jenkins-CI to enable installation from the package:

$sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

 
###Install Jenkins:
$ sudo yum install jenkins -y

##Enable the Jenkins service to start at boot:

$ sudo systemctl enable jenkins
// start Jenkins as a service:
$ sudo systemctl start jenkins

You can check the status of the Jenkins service using the command:

$ sudo systemctl status jenkins
```
### Step 6
Configuring Jenkins
Jenkins is now installed and running on your Server. To configure Jenkins:
1.  Connect using your servers Public IP  http://yourIPaddress:8080 from your browser. You will be able to access Jenkins through its management interface:
As prompted, enter the password found in /var/lib/jenkins/secrets/initialAdminPassword.
Use the following command to display this password:

``` 	
$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
<img width="337" alt="image" src="https://user-images.githubusercontent.com/10384951/217360785-e3243c32-f21a-42a2-9396-e249b75e56eb.png">

The Jenkins installation script directs you to the Customize Jenkins page > Click Install suggested plugins.
Once the installation is complete > the Create First Admin User will open. Enter your information, and then select Save and Continue.

 <img width="308" alt="image" src="https://user-images.githubusercontent.com/10384951/217360862-9c00baf3-a3ca-4f5e-bafc-ea3ef6ddc9ee.png">

On the left-hand side, select Manage Jenkins, and then select Manage Plugins. search & install

Select the Available tab, and then enter the following plugins
- Docker  plugin -install without restart
- Docker pipeline -> install without restart
- Warnings NG Install without restart
- Maven -install without restart
- Build pipeline install without restart
- GitHub Integration Plugin install without restart
- Junit plugin install without restart
- Parameterized trigger Plugin install without restart
Sample screenshot below, the checkbox next to  plugin, and then select Install without restart.

  
<img width="333" alt="image" src="https://user-images.githubusercontent.com/10384951/217360925-d9db961b-22f2-4370-a773-05ca1837c8ba.png">

 
### To Create your First Job you need to first  point Jenkins to your applications installation path. For java, git, maven
 
#### From your dashboard >Manage jenkins>Global tool configuration
 <img width="421" alt="image" src="https://user-images.githubusercontent.com/10384951/217360985-06020cf7-f539-446b-be10-41b16973f1aa.png">

Run the command below on you terminal to get the java path
```
[root@vps-f6b31a15 ~]# echo $JAVA_HOME
/usr/lib/jvm/java-11-openjdk-11.0.17.0.8-2.el8_6.x86_64
```
On your Jenkins server interface select java
 
Us the  path above in your jenkins config 
 <img width="452" alt="image" src="https://user-images.githubusercontent.com/10384951/217361070-0efd4b63-b2fd-480e-a625-c5bc71ba9c2c.png">

On the global tool configuration page select Git
 ```
 [root@vps-f6b31a15 ~]# which git
 /bin/git
 ```
 <img width="452" alt="image" src="https://user-images.githubusercontent.com/10384951/217361179-65f1eaba-902f-4ff2-9f78-6a77f7b3b77b.png">

 //On your server run mvn –version  to retrieve the mvn version and  installation path as shown below
```
[root@vps-f6b31a15 ~]# mvn --version
Apache Maven 3.8.6 (84538c9988a25aec085021c365c560670ad80f63)
Maven home: /opt/maven
``` 
<img width="452" alt="image" src="https://user-images.githubusercontent.com/10384951/217361235-e042074f-db07-46ee-9c00-e4c0544c1533.png">

//if you search for a plugin and its not displayed got to installed section and search it may have been installed already by default on jenkins during the setup process.

Click on Apply then Save


### Jenkins ( CI ) job pipeline
                                                                     
### Step 1
Click on new item Select free style Jobs->create a name for the job -> click ok
 <img width="241" alt="image" src="https://user-images.githubusercontent.com/10384951/217361327-31f1baa9-45dc-4372-a7dd-16299cad9e42.png">

  
### Step 2
 
<img width="388" alt="image" src="https://user-images.githubusercontent.com/10384951/217361365-78af6564-4716-47e7-b5b3-226903dc07fc.png">

### Step 3
Select source code management->select git -> paste the repository url shown below :https://github.com/learndevops-083/samplejavaapp.git
No credentials are needed 
Leave the branch as master
 <img width="391" alt="image" src="https://user-images.githubusercontent.com/10384951/217361434-7f2b982e-7fcd-4f51-ab32-a1d237611e22.png">
<img width="452" alt="image" src="https://user-images.githubusercontent.com/10384951/217361454-30da413c-5c55-41dd-ba41-c89067442248.png">

 
### Step 4 
Click on build steps -> invoke top-level maven targets-> select your maven version -> goal :package

 <img width="452" alt="image" src="https://user-images.githubusercontent.com/10384951/217361484-bc11f0be-ddbd-4128-8599-16c5508c8ad6.png">

Click on apply->save

### Step 5
When the Job display page is shown ->click on build now to build.
<img width="225" alt="image" src="https://user-images.githubusercontent.com/10384951/217361666-ce0617da-83f7-492f-b338-bffff20e4cc1.png">

When the job runs successfully you will see the display below under build history-> you can click on that to view the console output.
<img width="363" alt="image" src="https://user-images.githubusercontent.com/10384951/217361725-6dce887d-395a-43f9-add0-a28e23e6dec5.png">

We have successfully implemented a simple ci pipeline


### To enable jenkins to push our deployed war file to our tomcat server

### Step one
Go to Manage Jenkins > plugin manager >search for Deploy to container Plugin
Version  in available plugging >select install without restart

### Step 2 
//we need to carry out some configurations to in our tomcat server to enable a successful war file push.
```
$ sudo -i  
$ cd /opt/apache-tomcat-9.0.70/conf
```
//its recommended to create a tomcat user that will be used to deploy the war file.
Copy  the line below in the section below and past as shown in the tomcat-users.xml file just before the closing tag for </tomcat-users>
```
<role rolename="manager-script"/>
<user username="deployer" password="deployer" roles="manager-script"/>
```
```
$ nano tomcat-users.xml
```
<img width="446" alt="image" src="https://user-images.githubusercontent.com/10384951/217362694-ed28e5a8-4d41-4eac-a3b2-ab33d06895ac.png">

//we need to add our public ip for our server so tomcat can make our push content from jenkins accessible via our browser
```
$ nano  /opt/apache-tomcat-9.0.70/webapps/manager/META-INF/context.xml
```
 add the public  ip of your jenkins/tomcat server   to this line in the file
From this
```
<Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1|"/>

To this note my public ip is added

<Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1|Publicip" />
```
````
$cd /opt/apache-tomcat-9.0.70/binde 
$ ./shutdown.sh
$ ./startup.sh
```
On your Jenkis console manage jenkins >credentials >click on global >add credentials
Select username and password  and fill the details for the tomcat user we created.

 <img width="452" alt="image" src="https://user-images.githubusercontent.com/10384951/217363009-1f3b4e10-7981-4cb7-b31d-ed62f3512b01.png">
<img width="400" alt="image" src="https://user-images.githubusercontent.com/10384951/217363056-25c1079c-c3b6-42e5-9da8-9b8de2fa0cf7.png">

 
Navigate back to the jenkins console select the last Maven compile Job > select configure
```
Select postbuild actions >select deploy  war/ear to container
War/EAR files :target/sampleapp.war
Context path:sampleapp
Containers:tomcat 9.X
Credentials :select the credentials created earlier 
Username:deployer
Password:deployer
Tomcat url:mypublicip:8083
 ```

Click apply then save .
On the console click build

![image](https://user-images.githubusercontent.com/10384951/217356271-d747c1fb-d5f1-462f-a8cd-7f575690965a.png)
