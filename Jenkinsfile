pipeline {
    agent any

    environment {
        COMPOSE_CMD = 'docker-compose'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'git@github.com:Prabhasgyawali/sample-todo.git'
            }
        }

        stage('Build Laravel Containers') {
            steps {
                sh "${COMPOSE_CMD} build"
            }
        }

        stage('Start Containers') {
            steps {
                sh "${COMPOSE_CMD} up -d"
            }
        }
    }
}
