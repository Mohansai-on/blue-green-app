pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-bluegreen-app"
        BLUE_PORT = "8084"
        GREEN_PORT = "8085"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    bat "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Deploy to Green Environment') {
            steps {
                script {
                    def greenContainer = bat(script: "docker ps -q -f name=green", returnStdout: true).trim()
                    if (greenContainer) {
                        bat "docker stop green && docker rm green"
                    }
                    bat "docker run -d --name green -p ${GREEN_PORT}:80 ${IMAGE_NAME}"
                }
            }
        }

        stage('Switch Traffic') {
            steps {
                script {
                    def blueContainer = bat(script: "docker ps -q -f name=blue", returnStdout: true).trim()
                    if (blueContainer) {
                        bat "docker stop blue && docker rm blue"
                    }
                    bat "docker rename green blue"
                }
            }
        }
    }
}
