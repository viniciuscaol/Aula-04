pipeline {
    agent any

    stages {
        
        stage ('Build Docker Image') {
            steps {
                script {
                    dockerapp = docker.build("viniciuscaol/aula04-kubenews:v${env.BUILD_ID}", '-f ./src/dockerfile ./src') 
                }
            }
        }

    }
}