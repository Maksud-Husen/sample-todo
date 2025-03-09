pipeline {
    agent any

    environment {
        COMPOSE_CMD = 'docker-compose'
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    checkout([$class: 'GitSCM',
                        branches: [[name: '*/master']],
                        userRemoteConfigs: [[ url: 'git@github.com:Prabhasgyawali/sample-todo.git',
                        ]]
                    ])
                }
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

        stage('Check Running Containers') {
            steps {
                sh "${COMPOSE_CMD} ps"
            }
        }

    }

    post {
        always {
            script {
                sh "${COMPOSE_CMD} logs || true"
            }
        }
    }
}
