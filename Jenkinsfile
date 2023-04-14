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

        stage('Deploy Kubernetes') {
            steps {
                withKubeConfig([credentialsId: 'kubeconfig']) {
                    sh 'kubectl apply -f ./k8s/deployment.yaml'
                }
            }
        }
    }
}
