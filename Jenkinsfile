pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-bluegreen-app"
        BLUE_PORT = "8088"
        GREEN_PORT = "8087"
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
                    bat '''
                        docker stop green || echo "Green container not running"
                        docker rm green || echo "Green container not found"
                        docker run -d --name green -p %GREEN_PORT%:80 %IMAGE_NAME%
                    '''
                }
            }
        }

        stage('Switch Traffic') {
            steps {
                script {
                    def blueContainer = bat(script: 'docker ps -q -f name=blue', returnStdout: true).trim()
                    if (blueContainer) {
                        bat "docker stop blue || echo Blue container not running"
                        bat "docker rm blue || echo Blue container not found"
                    } else {
                        echo "No existing blue container to stop/remove"
                    }
                    bat "docker rename green blue"
                }
            }
        }
    }
}
