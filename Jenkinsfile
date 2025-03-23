pipeline {
    agent any
    tools{
        maven 'maven_3_5_0'
    }

    stages {
        stage('Hello') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/maysharma123/devops_automation.git']])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image') {
            steps {
                script{
                    sh 'docker build -t mayanksharma12/devops-integrations .'
            }
        }
    }
        stage('Deploy container'){
            steps {
                script{
                    sh 'docker run -itd --name test -p 8081:8080 mayanksharma12/devops-integrations'
                }
            }
        }
        stage('Push image to hub'){
            steps {
                script{
                    withCredentials([string(credentialsId: 'mayanksharma12', variable: 'dockerhub')]) {
                    sh 'docker login -u mayanksharma12 -p ${dockerhub}' 
}
                    sh 'docker push mayanksharma12/devops-integrations'
            }
        }
    }
}
}
