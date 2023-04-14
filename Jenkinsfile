pipeline {
    agent any

    stages {
        stage('build Docker Image') {
            steps {
                script {
                    dockerapp = docker.build('ogrds/kube-news:${env.BUILD_ID}', '-f ./src/Dockerfile ./src')
                }
            }
        }
    }
}
