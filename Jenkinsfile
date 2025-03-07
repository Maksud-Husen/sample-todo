pipeline {
    agent any

    environment {
        COMPOSE_CMD = 'docker-compose'
        PROJECT_DIR = '/var/www/html/Todo' // Ensure it matches your Docker setup
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    git branch: 'main', credentialsId: 'your-ssh-key-id', url: 'git@github.com:Prabhasgyawali/sample-todo.git'
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

        stage('Run Laravel Migrations') {
            steps {
                sh "docker exec -it laravel_app php artisan migrate --force"
            }
        }
    }

    post {
        always {
            script {
                sh "${COMPOSE_CMD} logs"
            }
        }
        cleanup {
            sh "${COMPOSE_CMD} down --remove-orphans"
        }
    }
}
