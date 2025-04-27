pipeline {
    agent none
    environment {
        DOCKER_IMAGE = 'santi099/spring-petclinic' //profe me toco usar la imagen de mi compañero pq ami no me quizo crear y la del tutorial ya no existe 
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
        stage('Docker Push') {
            agent any
            steps {
                script {
                    // Use the Docker Hub credentials to authenticate and push
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerHub') {
                        def appImage = docker.image('santi099/spring-petclinic:latest')
                        appImage.push()
                    }
                }
            }
        }
    }
}






