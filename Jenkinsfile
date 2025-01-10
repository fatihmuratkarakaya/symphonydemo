pipeline {
    agent any

    environment {
        COMPOSER_HOME = "${env.WORKSPACE}/.composer"
        SONAR_TOKEN = credentials('jenkins-token') // Ensure 'jenkins-token' matches your Jenkins credential ID
    }

    stages {
        stage('Checkout') {
            steps {
                git(
                    url: 'https://github.com/symfony/demo.git',
                    branch: 'main',
                    poll: false
                )
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Use Composer Docker image to install dependencies
                    docker.image('composer:2').inside {
                        sh 'composer install --prefer-dist --no-interaction'
                    }
                }
            }
        }

        stage('Run Unit Tests') {
            steps {
                script {
                    // Use Composer Docker image to run PHPUnit
                    docker.image('composer:2').inside {
                        // Ensure that PHPUnit is installed via Composer
                        sh './bin/phpunit' // Adjust path if PHPUnit is located elsewhere
                    }
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Execute SonarScanner using Docker and connect to the correct network
                    docker.image('sonarsource/sonar-scanner-cli:latest').inside('--network lupo_devops-network') {
                        sh """
                            sonar-scanner \
                                -Dsonar.projectKey=SymfonyDemo \
                                -Dsonar.sources=. \
                                -Dsonar.host.url=http://sonarqube:9000 \
                                -Dsonar.login=${SONAR_TOKEN}
                        """
                    }
                }
            }        
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
        always {
            cleanWs()
        }
    }
}
