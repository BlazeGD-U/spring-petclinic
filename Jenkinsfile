pipeline {
    agent none
    stages {
        stage('Maven Install') {
            agent {
                docker {
                    image 'maven:3.8.6-openjdk-17-slim'
                    args '-u root'
                }
            }
            steps {
                sh 'mvn clean install'
            }
        }
    }
}




