pipeline {
    agent any

    environment {
        COMPOSE_CMD = 'docker compose'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'git@github.com:Maksud-Husen/sample-todo.git'              
            }
        }
        stage ('deleting old containers') {
            steps {
                sh "${COMPOSE_CMD} down || true"
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