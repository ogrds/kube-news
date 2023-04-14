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
                        dockerapp.push("${imageTag}")
                        dockerapp.push('latest')
                    }
                }
            }
        }

        stage('Deploy Kubernetes') {
            environment {
                tag_version = "${imageTag}"
            }

            steps {
                withKubeConfig([credentialsId: 'kubeconfig']) {
                    sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/deployment.yaml'
                    sh 'kubectl apply -f ./k8s/deployment.yaml'
                }
            }
        }
    }
}
