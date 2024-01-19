pipeline {
    agent any
     environment {
        // Define your remote server details
        remoteServer = '13.127.37.206'
        remoteUser = 'ubuntu'
        privateKeyName = credentials('awskeyansible')
        }
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
            agent any
            steps {
                script {

                        sh "ssh -o StrictHostKeyChecking=no -i ${privateKeyName} ubuntu@13.127.37.206 'docker login -u \$duser -p \$dpass'"
                        sh "ssh -o StrictHostKeyChecking=no -i ${privateKeyName} ubuntu@13.127.37.206 'docker run -t -id --name nodejs -p 3000:3000 vjyguvi/projectnodejs'"
                    }
            }
                        }
        }
}
