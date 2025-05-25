pipeline {
    agent any

    environment {
        COMPOSE_CMD = 'docker-compose'
    }

    stages {
        stage('Clone Repo') {
            steps {
                sh '''
                rm -rf sample-todo || true
                git clone https://github.com/Maksud-Husen/sample-todo.git
                '''
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
        stage('restarting app conterner') {
            steps {
                sh "docker restart laravel_app"
            }
        }
    }

    post {
        always {
            sh "${COMPOSE_CMD} logs || true"
        }
    }
}
