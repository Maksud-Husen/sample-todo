pipeline {
    agent any

    environment {
        COMPOSE_CMD = 'docker-compose'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'git@github.com:Prabhasgyawali/sample-todo.git'
            }
        }

        stage('Build & Start Containers') {
            steps {
                sh "${COMPOSE_CMD} up --build -d"
            }
        }

        stage('Check Running Containers') {
            steps {
                sh "${COMPOSE_CMD} ps"
            }
        }
    }

    post {
        always {
            sh "${COMPOSE_CMD} logs || true"
        }
    }
}
