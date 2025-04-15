pipeline {
    agent any

    environment {
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone the repository
                checkout scm
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    // Build backend and frontend Docker images
                    sh 'docker-compose build'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run tests (if you have tests defined)
                    sh 'docker-compose run backend npm test'
                    sh 'docker-compose run frontend npm test'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Start the application
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        always {
            // Clean up unused Docker resources
            sh 'docker system prune -f'
        }
    }
}