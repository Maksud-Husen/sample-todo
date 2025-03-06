// pipeline {
//     agent any

//     environment {
//         DEPLOY_DIR = '/var/www/html/'
//         REPO_URL = 'git@github.com:Prabhasgyawali/sample-todo.git'
//         BRANCH = 'master'
//     }

//     stages {
//         stage('Clone Repository') {
//             steps {
//                 git branch: env.BRANCH, url: env.REPO_URL
//             }
//         }
//         stage('Install Composer') {
//             steps {
//                 script {
//                     // Install Composer
//                     sh '''
//                         curl -sS https://getcomposer.org/installer | php
//                         sudo mv composer.phar /usr/local/bin/composer
//                     '''
//                 }
//             }
//         }

//         stage('Install Dependencies') {
//             steps {
//                 sh 'composer install --no-dev --prefer-dist'
//                 sh 'cp .env.example .env'
//                 sh 'php artisan key:generate'
//             }
//         }

//         stage('Run Tests') {
//             steps {
//                 sh 'php artisan test'
//             }
//         }

//         stage('Build Assets') {
//             steps {
//                 sh 'npm install && npm run build'
//             }
//         }

//         // stage('Set Permissions') {
//         //     steps {
//         //         sh 'chmod -R 775 storage bootstrap/cache'
//         //         sh 'chown -R www-data:www-data .'
//         //     }
//         // }

//         stage('Deploy') {
//             steps {
//                 sh "sudo rsync -av --delete ./ ${DEPLOY_DIR}"
//             }
//         }

//         stage('Restart Nginx & PHP') {
//             steps {
//                 sh 'sudo systemctl restart nginx'
//                 sh 'sudo systemctl restart php8.3.6-fpm'
//             }
//         }
//     }
// }





pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'git@github.com:Prabhasgyawali/sample-todo.git'
            }
        }

        stage('Install Composer') {
            steps {
                script {
                    sh '''
                        if [ ! -f "$WORKSPACE/composer" ]; then
                            curl -sS https://getcomposer.org/installer | php
                            mv composer.phar $WORKSPACE/composer
                            chmod +x $WORKSPACE/composer
                        fi
                    '''
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    sh '$WORKSPACE/composer install --no-dev --prefer-dist'
                }
            }
        }

        stage('Run Tests') {
            steps {
                sh 'php artisan test'
            }
        }

        stage('Build Assets') {
            steps {
                sh 'npm install && npm run prod'
            }
        }

        stage('Deploy') {
             steps {
                sh '''
                    sudo cp -r $WORKSPACE/* /var/www/html/
                    sudo chown -R www-data:www-data /var/www/html
                '''
            }
        }

        stage('Restart Nginx & PHP') {
            steps {
                script {
                    
                        sh 'echo "Restarting Nginx & PHP..."'
                        //Uncomment the following line if needed
                         sh 'sudo systemctl restart nginx php-fpm'
                    
                }
            }
        }
    }

    post {
        success {
            echo ' Deployment successful!'
        }
        failure {
            echo 'Pipeline failed. Check logs for errors.'
        }
    }
}
