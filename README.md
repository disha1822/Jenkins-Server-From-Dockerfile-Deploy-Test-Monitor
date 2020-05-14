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

I have used Red Hat 8 as my base OS and made a separate directory for this project
` mkdir /task2`
inside it created one Dockerfile for Jenkins server ` vim Dockerfile` :
![dockerfile](https://github.com/disha1822/Jenkins-Server-From-Dockerfile-Deploy-Test-Monitor/blob/master/dockerfile.png?raw=true)
then to build the **docker image** from it run the command `docker build -t jenkins:v1 .`. It will build the image for Jenkins from the Dockerfile in the current directory and after an successful build we will get outout like this:
![build]()
Now we can check the Jenkins image by ` docker images | grep jenkins` the we can use this image to launch our Jenkins server :
` docker run  -it --privileged -v /:/host -p 1234:8080 --name jkserver jenkins:v1` 
`--privileged` is used to give special access to run docker commands in host OS through jenkins server.
`-v /:/host` mounting is done for this reason only.
`-p 1234:8080` by exposing port we can access Jenkins server from http://localhost:1234
For first time Jenkins will ask an Initial Admin Password which we can find in the running jenkins container :
![passwd]()
