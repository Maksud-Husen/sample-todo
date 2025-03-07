pipeline {
    agent any

    environment {
        COMPOSE_CMD = 'docker-compose'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/your-repo/laravel-docker.git'
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

        stage('Run Migrations & Seed Database') {
            steps {
                sh "${COMPOSE_CMD} exec -T app php artisan migrate --seed"
            }
        }

        stage('Test Laravel App') {
            steps {
                sh 'curl -I http://localhost:8082'
            }
        }

        // stage('Cleanup') {
        //     steps {
        //         sh "${COMPOSE_CMD} down"
        //     }
        // }
    }
}
