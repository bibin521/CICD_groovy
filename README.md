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
step2
create new project with pipeline
step3(creating groovy script)


#Configuration

node {
    stage('git checkout'){
       git 'https://github.com/bibin521/Jenkins-Docker-Project.git' 
    }
    
    stage('Docker build image'){  
        sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID . '
        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID bibin521/$JOB_NAME:v1.$BUILD_ID'
        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID bibin521/$JOB_NAME:latest'
        
    }
    
    stage('pushing images to dockerhub'){
        withCredentials([string(credentialsId: 'DockerPasswd', variable: 'DockerPasswd')]) {
          sh "docker login -u bibin521 -p ${DockerPasswd}"
          sh 'docker image push bibin521/$JOB_NAME:v1.$BUILD_ID'
          sh 'docker image push bibin521/$JOB_NAME:latest'
          sh 'docker image rm bibin521/$JOB_NAME:v1.$BUILD_ID'
          sh 'docker image rm bibin521/$JOB_NAME:latest'
          sh 'docker image rm $JOB_NAME:v1.$BUILD_ID'
}
    stage('docker container deployment'){
         def docker_run = 'docker container run -d --name scriptedcontainer -p 80:80 bibin521/dockr-groovy-webapp'
         def docker_rmv_container = 'docker rm -f scriptedcontainer'
         def docker_rmi = 'docker rmi -f bibin521/dockr-groovy-webapp'
         sshagent(['webserver']) {
    sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.4.101 ${docker_rmv_container}"
    sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.4.101 ${docker_rmi}"
    sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.4.101 ${docker_run}"
}
         
    }
    
        
    }
}






