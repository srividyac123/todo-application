pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/srividyac123/todo-application.git'
        GITHUB_CREDENTIALS = 'credsgitnew'
        DOCKERHUB_CREDENTIALS = 'docker'
    }
    stages {
      stage('chekout') {
        steps {
           script{
             git credentialsId: "${GITHUB_CREDENTIALS}", url: "${GIT_REPO}"
           }
        }
      }
      stage('build') {
        steps {
            sh 'mvn clean install -DskipTests'
        }
      }
      stage('build docker image') {
        steps {
          script {
            docker.build("${IMAGE_NAME}:latest")
          }
        }
      }
      stage('push docker image') {
        steps {
          script {
            docker.withRegistry('https://registry.hub.docker.com', "${DOCKERHUB_CREDENTIALS}") {
              docker.image("${IMAGE_NAME}:latest").push()
            }
          }
        }
      }
      stage('deploy with docker compose') {
        steps {
          script {
            sh 'docker-compose -f docker-compose.yaml up -d'
          }
        }
      }
      stage('verify services') {
        steps {
          script {
            def servicesStatus = sh(script: 'docker-compose -f docker-compose.yaml ps', resturnStdout: true).trim()
            if (serviceStatus.contains("up")) {
                echo "yes"
            } else {
                error "not running"
            }
          }
        }
      }
    }
    post [
        always {
            script {
                sh 'docker-compose -f docker-compose.yaml down'
            }
        }
    ]
}
