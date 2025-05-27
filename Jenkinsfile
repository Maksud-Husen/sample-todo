pipeline {
    agent any

    environment {
        COMPOSE_CMD = 'docker compose'
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Clone Repo') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/master']],
                    userRemoteConfigs: [[
                        url: 'git@github.com:Maksud-Husen/sample-todo.git',
                        credentialsId: 'git'
                    ]]
                ])
            }
        }

        stage('Delete Old Containers') {
            steps {
                sh "${env.COMPOSE_CMD} down || true"
            }
        }

        stage('Build & Start Containers') {
            steps {
                sh "${env.COMPOSE_CMD} up --build -d"
            }
        }

        stage('Check Running Containers') {
            steps {
                sh "${env.COMPOSE_CMD} ps"
            }
        }
        stage('restarting an contener') {
            steps {
                sh "docker restart laravel_app"
            }
        }
    }

    post {
        always {
            script {
                sh "${env.COMPOSE_CMD} logs || true"
            }
        }
    }
}
