pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "ghcr.io/LeonArchie/TEST_PUSH_DOKER:latest"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Login to GitHub Packages') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'github-docker-credentials',
                    usernameVariable: 'GITHUB_USERNAME',
                    passwordVariable: 'GITHUB_TOKEN'
                )]) {
                    sh "echo ${GITHUB_TOKEN} | docker login ghcr.io -u ${GITHUB_USERNAME} --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://ghcr.io', 'github-docker-credentials') {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }
    }
}