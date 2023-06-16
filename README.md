# CI &CD docker based application using Jenkins Pipeline scripts(Groovy)
Complete CICD DevOps project | Git, Dockerhub, Dockerhost,Jenkins|Deploying container on web-server


![Screenshot from 2023-06-16 09-17-52](https://github.com/bibin521/CICD_groovy/assets/115148672/4bbbe78a-ad92-4fc9-a97b-d1f61018e477)

# WORK_FLOW
The  end users will writing the docker file and push it to the github repository .Once this completed, the jenkins will start building docker imaged based on the docker file. Once the jenkins build the docker images based on the docker file jenkins itself will push the docker images to the docker hub. we will enable the passwordless authentication on application server. Once the lateset image will push the jenkins itselt ssh to the web app server and it will create container based on the latest image from dockerhub 

# steps
1. launch 2 ec2(jenkins+webapp)
2. ensure docker installed on both server
3. install jenkins on jenkins server
4. create pipeline & mention stages
   a)git checkout
   b) build docker image
   c) push docker image to dockerhub
   d) run container on webapp_server by pulling the same image from dockerhub
5. resolve the issues if any comes

step1
logged into the jenkins and install ssh agent plugin



