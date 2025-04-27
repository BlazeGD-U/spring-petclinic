pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'blazegd/spring-petclinic'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/BlazeGD-U/spring-petclinic.git',
                        credentialsId: 'github-token'
                    ]]
                ])
            }
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
            script{
                if (!fileExists('Dockerfile')) {
                        error("Dockerfile no encontrado")
                    }
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }
        }
    }
    }
}






