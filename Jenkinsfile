pipeline {
    agent any

    environment {
        DEPLOY_DIR = '/var/www/html/'
        REPO_URL = 'git@github.com:Prabhasgyawali/sample-todo.git'
        BRANCH = 'master'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: env.BRANCH, url: env.REPO_URL
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'composer install --no-dev --prefer-dist'
                sh 'cp .env.example .env'
                sh 'php artisan key:generate'
            }
        }

        // stage('Run Tests') {
        //     steps {
        //         sh 'php artisan test'
        //     }
        // }

        stage('Build Assets') {
            steps {
                sh 'npm install && npm run build'
            }
        }

        // stage('Set Permissions') {
        //     steps {
        //         sh 'chmod -R 775 storage bootstrap/cache'
        //         sh 'chown -R www-data:www-data .'
        //     }
        // }

        stage('Deploy') {
            steps {
                sh "rsync -av --delete ./ ${DEPLOY_DIR}"
            }
        }

        stage('Restart Nginx & PHP') {
            steps {
                sh 'systemctl restart nginx'
                sh 'systemctl restart php8.3.6-fpm'
            }
        }
    }
}
