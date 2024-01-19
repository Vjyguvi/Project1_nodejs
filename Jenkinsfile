pipeline {
    agent any
stages {
       stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/Vjy05git/Project1_nodejs.git']])
            }
        }

        stage('Build Image') {
            steps {
                script {
                    
                    sh "docker build -t vjyguvi/projectnodejs ."
                }
            }
        }

        stage('Push Image') {
            steps {
                script { 
                     withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dpass', usernameVariable: 'duser')]) {
    sh "docker login -u \$duser -p \$dpass"
    sh "docker push vjyguvi/projectnodejs"
}
                    
                }
            }
        }

        stage('Deploy') {
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
            }
        }
}
}
