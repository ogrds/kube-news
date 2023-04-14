pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    imageTag = "v${env.BUILD_ID}"
                    dockerapp = docker.build("ogrds/kube-news:${imageTag}", '-f ./src/Dockerfile ./src')
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry("https://registry.hub.docker.com", "dockerhub") {
                        dockerapp.push('latest')
                        dockerapp.push("${imageTag}")
                    }
                }
            }
        }
    }
}
