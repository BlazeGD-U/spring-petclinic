pipeline {
    agent none
    environment {
        DOCKER_IMAGE = 'santi099/spring-petclinic' //profe me toco usar la imagen de mi compañero pq ami no me quizo crear y la del tutorial ya no existe 
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
            when {
                branch 'main'
            }
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-hub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${DOCKER_IMAGE}:${DOCKER_TAG}
                    '''
}
            }
        }

    post {
        always {
            echo "Pipeline completado - Limpieza de workspace"
        }
    }
}






