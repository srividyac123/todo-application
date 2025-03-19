pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
    }
    stages{
        stage('Clone Repository'){
            steps{
                git branch: 'master', url: ''
            }
        }
        stage('builds with maven'){
            steps{
                sh 'mvn clean package -Dskiptests'
            }
        }
        stage('Build Docker Image'){
            steps{
                sh 'docker build -t todo-application-image:latest .'
            }
        }
        stage('push image to docker hub'){
            steps{
                sh 'docker login -u srividyac09 -p 12vidya12'
                sh 'docker tag todo-application-image:latest srividyac09/odo-application-image:latest'
                sh 'docker push srividyac09/todo-application-image:latest'
            }
        }
        stage('Verify services'){
            steps{
                sh 'docker ps'
            }
        }
        stage('Clean Workspace'){
            steps{
                sh 'rm -rf *'
            }
        }
    }
    post {
        success {
            echo 'Build and deployment Successful'
        }
        failure {
            echo 'Build and Deployment Failed'
        }
    }
}
