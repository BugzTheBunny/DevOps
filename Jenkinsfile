pipeline {
    agent any
    environment {
        PATH = "$PATH:/usr/local/bin"
    }
    stages {
        stage('run webapp container') {
            steps {
                sh """
                docker-compose up -d
                """
            }
        }
    }
}