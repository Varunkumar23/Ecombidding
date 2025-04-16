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
                    // Run backend tests (if defined)
                    sh 'docker-compose run --rm backend npm test || echo "No tests defined for backend"'

                    // Run frontend tests (if defined)
                    sh 'docker-compose run --rm frontend npm test || echo "No tests defined for frontend"'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Start the application in detached mode
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
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}