pipeline {
    agent any

    environment {
        // Имя Docker-образа в GitHub Packages (замените на свои значения)
        DOCKER_IMAGE = "ghcr.io/leonarchie/test_push_doker:latest"
        // Реестр GitHub Packages
        GITHUB_REGISTRY = "ghcr.io"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm  // Клонируем репозиторий
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Собираем Docker-образ
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Login to GitHub Packages') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'GitHubPackages_CRED',  // Имя кредов в Jenkins
                    usernameVariable: 'GITHUB_USER',
                    passwordVariable: 'GITHUB_TOKEN'
                )]) {
                    // Логинимся в GitHub Container Registry
                    sh """
                        echo "Logging in to GitHub Packages..."
                        echo ${GITHUB_TOKEN} | docker login ${GITHUB_REGISTRY} \
                            -u ${GITHUB_USER} \
                            --password-stdin
                    """
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Пушим образ в GitHub Packages
                    docker.withRegistry("https://${GITHUB_REGISTRY}", 'GitHubPackages_CRED') {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }
    }

    post {
        always {
            // Очистка: удаляем Docker-образ после успешной публикации
            script {
                sh "docker rmi ${DOCKER_IMAGE} || true"
            }
        }
    }
}