pipeline {
    agent any
    tools{
        maven 'maven_3_5_0'
    }
    stages{
        stage('Build Maven'){
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/maysharma123/devops_automation.git']])
                sh 'mvn clean install'
            }
        }
        stage('Docker image build'){
            steps {
                script{
                    sh 'docker rm -f $(docker ps -a -q)'
                    sh 'docker rmi -f $(docker images -a -q)'
                    sh 'docker build -t mayanksharma12/devops-intergration .'
                }
            }
        }
        stage('docker container create'){
            steps {
                script{
                    sh 'docker run -itd --name test -p 8081:8080 mayanksharma12/devops-intergration'
                }
            }
        }
        stage('Image push dockerhub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerpwd')]) {
                    sh 'docker login -u mayanksharma12 -p ${dockerpwd}'
}
                    sh 'docker push mayanksharma12/devops-intergration'                    
                }
            }
        }
    }
}
