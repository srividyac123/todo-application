pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/srividyac123/todo-application.git'
        GITHUB_CREDENTIALS = 'credsgitnew'
        DOCKERHUB_CREDENTIALS = 'docker-hub-credentials'
        IMAGE_NAME = 'srividyac09/todo-application-image'
    }
    stages {
      stage('chekout') {
        steps {
             git credentialsId: "${GITHUB_CREDENTIALS}", url: "${GIT_REPO}"
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
             withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS}" , usernamevariable: "DOCKER_USER, passwordVariable: "test123123")]) 
            {
              sh "echo test123123 | docker login -u ${DOCKER_USER} --password-stdin"
              sh "docker push ${IMAGE_NAME}:latest"
          }
        }
      }
      stage('deploy with docker compose') {
        steps {
          script {
             'docker-compose -f docker-compose.yaml up -d'
          }
        }
      }
      stage('verify services') {
        steps {
          script {
            def servicesStatus = sh(script: 'docker-compose -f docker-compose.yaml ps', returnStdout: true).trim()
            if (servicesStatus.contains("up")) {
                echo "yes"
            } else {
                error "not running"
            }
          }
        }
      }
    }
    post {
        always {
           script {
               'docker-compose -f docker-compose.yaml down'
            }
        }
}

}
