pipeline {
    agent any

    environment {
        COMPOSE_CMD = 'docker compose'
    }

    stages {
        stage('Checkout Repo') {
            steps {
                checkout scm
            }
        }

        stage('Remove Old Containers') {
            steps {
                dir('sample-todo') {
                    sh "${COMPOSE_CMD} down || true"
                }
            }
        }

        stage('Build & Start Containers') {
            steps {
                dir('sample-todo') {
                    sh "${COMPOSE_CMD} up --build -d"
                }
            }
        }

        stage('Check Running Containers') {
            steps {
                dir('sample-todo') {
                    sh "${COMPOSE_CMD} ps -a"
                }
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
