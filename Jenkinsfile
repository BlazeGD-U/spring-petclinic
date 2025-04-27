#!groovy
pipeline {
    agent none
    stages {
        stage('Maven Install') {
            agent {
                docker {
                    image 'maven:3.8.6' // <-- versión más moderna
                    args "-v ${env.WORKSPACE}:/workspace" // <-- montar el workspace
                }
            }
            steps {
                dir('/workspace') {
                    sh 'mvn clean install'
                }
            }
        }
    }
}


