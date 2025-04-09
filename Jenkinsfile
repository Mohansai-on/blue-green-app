pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('my-bluegreen-app')
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    docker.image('my-bluegreen-app').run('-d -p 8084:80')
                }
            }
        }
    }
}
