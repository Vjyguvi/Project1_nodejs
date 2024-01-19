pipeline {
    agent any
     environment {
        // Define your remote server details
        remoteServer = '3.111.30.67'
        remoteUser = 'ubuntu'
        privateKeyName = credentials('sshkeyansible')
        }
stages {
       stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/Vjyguvi/Project1_nodejs.git']])
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

                   sshagent(['sshkeyansible']) {
                        sh """ssh -o StrictHostKeyChecking=no -i ${privateKeyName} ubuntu@3.111.30.67 'docker login -u ${duser} -p ${dpass}'"""
                    
                    sh """ssh -o StrictHostKeyChecking=no -i ${privateKeyName} ubuntu@3.111.30.67 'docker run -t -id --name nodejs -p 3000:3000 vjyguvi/projectnodejs'"""
                       
                    }
            }
                        }
        }
}
}
