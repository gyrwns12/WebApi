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
                sh 'docker build -t $DOCKER_IMAGE:latest -f WebApi/Dockerfile WebApi/'  // WebApi 내부에서 빌드
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
                    sh 'kubectl apply -f WebApi/webapi-deployment.yaml'  // WebApi 내부에서 적용
                    sh 'kubectl apply -f WebApi/webapi-service.yaml'
                    sh 'kubectl rollout restart deployment webapi'
                }
            }
        }
    }
}