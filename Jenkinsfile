pipeline {
    agent none
    environment {
        DOCKER_IMAGE = 'shanem/spring-petclinic'
        DOCKER_TAG = 'latest'
    }
    stages {
        stage('Maven Install') {
            agent {
                docker {
                    image 'maven:3.9.6-eclipse-temurin-17'
                    args '-u root'
                }
            }
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Docker Build') {
            agent any
            steps {
                script {
                    if (!fileExists('Dockerfile')) {
                        error("Dockerfile no encontrado")
                    }
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }
    }
}






