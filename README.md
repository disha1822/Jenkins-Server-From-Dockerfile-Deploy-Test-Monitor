# Jenkins-Server-From-Dockerfile-Deploy-Test-Monitor

# Home Task Given by Mr. Vimal Daga Sir Under DevOps Assembly Lines Training

# Task Overview :
This task has the following series of things to be done :

* 1.	Create container image that’s has Jenkins installed  using dockerfile 
* 2.	When we launch this image, it should automatically starts Jenkins service in the container.
* 3.	Create a job chain of job1, job2, job3 and  job4 using build pipeline plugin in Jenkins 
* 4. Job1 : Pull  the Github repo automatically when some developers push repo to Github.
* 5. Job2 : By looking at the code or program file, Jenkins should automatically start the respective language interpreter install image        container to deploy code ( eg. If code is of  PHP, then Jenkins should start the container that has PHP already installed ).
* 6.	Job3 : Test your app if it  is working or not.
* 7.	Job4 : if app is not working , then send email to developer with error messages.
* 8.	Create One extra job job5 for monitor : If container where app is running. fails due to any reson then this job should automatically       start the container again.

# Software Required :
Here we have integrated :
* Jenkins 
* Git
* Docker

![integrate](https://i2.wp.com/www.techrunnr.com/wp-content/uploads/2019/01/gitdockerjenkins.png?fit=248%2C203&ssl=1)

# Project Description :

## 1. Jenkins Server from Dockerfile:

#### Creating Dockerfile :

I have used Red Hat 8 as my base OS and made a separate directory for this project
` mkdir /task2`
inside it created one Dockerfile for Jenkins server ` vim Dockerfile` :
![dockerfile](https://github.com/disha1822/Jenkins-Server-From-Dockerfile-Deploy-Test-Monitor/blob/master/dockerfile.png?raw=true)

#### Build Image :

Then to build the **docker image** from it run the command `docker build -t jenkins:v1 .`. 
It will build the image for Jenkins from the Dockerfile in the current directory and after an successful build we will get outout like this:

![build](https://github.com/disha1822/Jenkins-Server-From-Dockerfile-Deploy-Test-Monitor/blob/master/build.png?raw=true)

#### Launching Jenkins Server:
 
Now we can check the Jenkins image by ` docker images | grep jenkins` 
Then we can use this image to launch our Jenkins server :
`docker run  -it  --privileged  -v /:/host  -p 1234:8080  --name  jkserver  jenkins:v1`

`--privileged` is used to give special access to run docker commands in host OS through jenkins server.

`-v /:/host` mounting is done for this reason only.

`-p 1234:8080` by exposing port we can access Jenkins server from http://localhost:1234

For first time Jenkins will ask an Initial Admin Password which we can find in the running jenkins container :
![passwd](https://github.com/disha1822/Jenkins-Server-From-Dockerfile-Deploy-Test-Monitor/blob/master/jkstart.png?raw=true)

Now we will do the required settings and install suggested plugins in Jenkins.

## 2. Jobs in Jenkins
Now we will start with our Jobs in Jenkins :

  * **Job1(Copy Github Code) :**
 
    Whenever Developer pushes something new in Github, Job1 coppies the code from Github to a directory inside the Jenkins server       container
     ![a](https://github.com/disha1822/Jenkins-Server-From-Dockerfile-Deploy-Test-Monitor/blob/master/job1_1.png?raw=true)
     ![b](https://github.com/disha1822/Jenkins-Server-From-Dockerfile-Deploy-Test-Monitor/blob/master/job1_2.png?raw=true)
     ![c](https://github.com/disha1822/Jenkins-Server-From-Dockerfile-Deploy-Test-Monitor/blob/master/job1_3.png?raw=true)
  
  * **Job2(Deploy Container) :**
  
     Successful build of Job1 will trigger Job2 and it will launch the recpective container for the code of the developer.
     Here I have taken example of webpages so I have used apache webserver.
  
     ![d](https://github.com/disha1822/Jenkins-Server-From-Dockerfile-Deploy-Test-Monitor/blob/master/job2_1.png?raw=true)
     ![e](https://github.com/disha1822/Jenkins-Server-From-Dockerfile-Deploy-Test-Monitor/blob/master/job2_2.png?raw=true)
  
  * **Job3(Testing Container) :**
  
     After successful build of Job2 will trigger Job3 and it tests if the deployed container is working or not.It will be successfully built if the deployed container fails and then trigger Job4 for giving notification to the developer.
     
     ![f](https://github.com/disha1822/Jenkins-Server-From-Dockerfile-Deploy-Test-Monitor/blob/master/job3_1.png?raw=true)
     ![g](https://github.com/disha1822/Jenkins-Server-From-Dockerfile-Deploy-Test-Monitor/blob/master/job3_2.png?raw=true)
     
  * **Job4(Sending Notification) :**
  
     It sends email notification to the developer for unsuccessful build, so that developer can correct the code.
     
     ![h](https://github.com/disha1822/Jenkins-Server-From-Dockerfile-Deploy-Test-Monitor/blob/master/job4_1.png?raw=true)
     ![i](https://github.com/disha1822/Jenkins-Server-From-Dockerfile-Deploy-Test-Monitor/blob/master/job4_2.png?raw=true)
     
     For this to send email successfully we need to do the following configurations in Jenkins:
     
     **Manage Jenkins -> Configure System -> E-mail Notification**
     ![j](https://github.com/disha1822/Jenkins-Server-From-Dockerfile-Deploy-Test-Monitor/blob/master/config.png?raw=true)
     
     You can still get error sending e-mail. In that case :
     
     Open this file **/etc/sysconfig/jenkins** and change a line of **JENKINS_JAVA_OPTIONS** to this -
     
     `JENKINS_JAVA_OPTIONS="-Djava.awt.headless=true -Dmail.smtp.starttls.enable=true -Dmail.smtp.ssl.protocols=TLSv1.2"`
     
     After this restart your jenkins service
     
     
     For successfully sent E-mail developer will get e-mail like this :
     
     ![mail](https://github.com/disha1822/Jenkins-Server-From-Dockerfile-Deploy-Test-Monitor/blob/master/notification.jpg?raw=true)
  
  * **Job5(Monitoring Container) :**
  
     It will check every minute if the deployed container is up or not. If by any case the container is shut down it will relaunch it.
     
     ![k](https://github.com/disha1822/Jenkins-Server-From-Dockerfile-Deploy-Test-Monitor/blob/master/job5_1.png?raw=true)
     ![l](https://github.com/disha1822/Jenkins-Server-From-Dockerfile-Deploy-Test-Monitor/blob/master/job5_2.png?raw=true)
     
     
## 3. Pipeline looks like :

![m](https://github.com/disha1822/Jenkins-Server-From-Dockerfile-Deploy-Test-Monitor/blob/master/pipeline.png?raw=true)
