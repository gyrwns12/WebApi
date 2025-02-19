pipeline {
    agent any
    environment {
        DOCKER_CREDENTIAL_ID = 'docker-access-token'
        DOCKER_IMAGE = 'hyeokjunyun/webapi'
    }
    stages {
        stage('Clone') {
            steps {
                git url: 'https://github.com/gyrwns12/webapi.git', branch: 'master'
            }
        }
        stage('Build') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }
        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: env.DOCKER_CREDENTIAL_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                    sh 'docker push $DOCKER_IMAGE:latest'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh 'kubectl apply -f webapi-deployment.yaml'
                    sh 'kubectl apply -f webapi-service.yaml'
                }
            }
        }
    }
}