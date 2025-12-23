pipeline {
    agent any
    tools {
        maven 'maven_3_5_0'
    }

     stages {
        stage('Build Maven') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/maysharma123/devops_automation.git']])
                sh 'mvn clean install'
            }
        }
        stage('Docker build image'){
            steps{
                script{
                    sh 'docker rm -f $(docker ps -aq)'
                    sh 'docker rmi -f $(docker images -aq)'
                    sh 'docker build -t mayanksharma12/devops-intergation .'
                }
            }
        }
        stage('docker container creation'){
            steps{
                script{
                    sh 'docker run -itd --name con1 -p 8081:8080 mayanksharma12/devops-intergation'
                }
            }
        }
        stage('image push dockerhub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhub')]) {
                    sh 'docker login -u mayanksharma12 -p ${dockerhub}'
                    }
                    sh 'docker push mayanksharma12/devops-intergation'
                }
            }
        }
     }
}
