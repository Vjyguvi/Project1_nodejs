# Project 1: Continuous Integration and Deployment of Node.js Application with Jenkins, AWS EC2 and Docker.
## Set up an EC2 instance:
### Create EC2 instance (jenkins Master-Slave)
>> ![Screenshot instance](https://github.com/vijaygiitt/Project1_nodejs/assets/157097326/3bd3260d-cfaf-4a15-aed5-b678d3c5e0c6)
## Install and configure Jenkins:

+ Install Jenkins on the EC2 instance and start the Jenkins service.
  ![Screenshot jenkins status'](https://github.com/vijaygiitt/Project1_nodejs/assets/157097326/3760ce45-26b3-4343-a598-b6dfa83694ea)
+ Create a new Jenkins job to build the Node.js application and push it to a Docker registry.
  ![Screenshot projectcreated](https://github.com/vijaygiitt/Project1_nodejs/assets/157097326/87d56a69-58c8-4d8a-a0bb-76fbbb82df67)
+ Jenkins Slave added (Node)
  ![Screenshot node jenkins](https://github.com/vijaygiitt/Project1_nodejs/assets/157097326/44a266da-9c9c-4151-bd1a-3cf52ef587d9)
## Build the Docker image:

+ Use the Dockerfile to build a Docker image of the Node.js application.
  ![Screenshot dockerfile](https://github.com/vijaygiitt/Project1_nodejs/assets/157097326/cee65ad2-a2a1-4d40-9c56-cfe95907f443)
## Push the Docker image to the registry:
+ Use Jenkins to push the Docker image to a Docker registry (e.g. Docker Hub).
  ```bash
    withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dpass', usernameVariable: 'duser')]) {
    sh "docker login -u \$duser -p \$dpass"
    sh "docker push vjyguvi/projectnodejs"
![Screenshot dockerhub](https://github.com/vijaygiitt/Project1_nodejs/assets/157097326/8c9ea3b3-8540-4e99-8a0d-2dc731fb71f9)

## Deployment:
+ Use Jenkins to SSH into the EC2 instance and pull the Docker image from the registry.
  ```bash
    agent {
                node {
                    
                    label 'nodejs'
                }
            }
            steps {
               script { 
                     withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dpass', usernameVariable: 'duser')]) {
                   sh "docker login -u \$duser -p \$dpass"      
                   sh "docker run -t -id --name nodejs -p 3000:3000 vjyguvi/projectnodejs"
                    }   
                }

+ Start a new Docker container on the EC2 instance using the Docker image.
  ![Screenshot container status](https://github.com/vijaygiitt/Project1_nodejs/assets/157097326/25450569-e905-40bd-9de2-a966d10ad25e)
## Verify the deployment:

+ Access the application in a web browser to confirm that it is running correctly.
  ![Screenshot 2024-01-19 output](https://github.com/vijaygiitt/Project1_nodejs/assets/157097326/555735d9-992f-4154-ba8f-0a3e1668d6f3)

  
