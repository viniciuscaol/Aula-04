pipeline {
    agent any

    stages {
        
        stage ('Build Docker Image') {
            steps {
                script {
                    dockerapp = docker.build("viniciuscaol/aula04-kubenews:${env.BUILD_ID}", '-f ./src/dockerfile ./src') 
                }
            }
        }
    
        stage ('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }                        
                }
            }
        }

        stage ('Deploy Kubernet') {
            environment {
                tag_version = "${env.BUILD_ID}"
            }
            steps {
                withKubeConfig ([credentialsId: 'kubeconfig']) {
                    sh 'sed -i "s/{{TAG}}/$tag_version/g" ./deployment.yaml'
                    sh 'kubectl apply -f ./deployment.yaml'
                }
            }
        }
    }

}