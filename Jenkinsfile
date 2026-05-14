pipeline {
    agent any

    tools {
        maven "maven_3_5_0"
    }
    stages {
        stage('Build Maven') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/maysharma123/devops_automation.git']])
                sh 'mvn clean install'
            }
        }
        stage ('Docker build image') {
            steps {
                script {
                    sh 'docker rm -f $(docker ps -aq)'
                    sh 'docker rmi -f $(docker images -aq)'
                    sh 'docker build -t mayanksharma12/myimage .'
                }
            }
        }
        stage ('Docker build Container') {
            steps {
                script {
                    sh 'docker run -itd --name mycon -p 8081:8080  mayanksharma12/myimage '
                }
            }
        }
 stage ('Docker Push Container') {
    steps {
        script {

            withCredentials([usernamePassword(
                credentialsId: 'dockerhub-creds',
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_PASS'
            )]) {

                sh '''
                echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                docker push mayanksharma12/myimage
                '''
            }
        }
    }
}
    }
}
