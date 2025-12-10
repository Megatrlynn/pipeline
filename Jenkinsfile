pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'username/my-web-app'
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
    }

    stages {

        stage('Build') {
            steps {
                echo "Building the project..."
                bat 'dir'   // Windows agent
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying..."
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Use Docker TCP endpoint to avoid context issues
                    docker.withServer('tcp://localhost:2375') {
                        dockerImage = docker.build("${DOCKER_IMAGE}:latest")
                        docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                            dockerImage.push('latest')
                        }
                    }
                }
            }
        }

        stage('Deploy to Local Docker Host') {
            steps {
                bat '''
                    docker rm -f my-web-app || exit 0
                    docker run -d --name my-web-app -p 8080:80 username/my-web-app:latest
                '''
            }
        }

    }

    post {
        success {
            echo "Pipeline completed successfully! 🎉"
        }
        failure {
            echo "Pipeline failed ❌ Check logs."
        }
    }
}
