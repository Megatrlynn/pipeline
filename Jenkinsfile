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
                sh 'ls -la'   // use bat 'dir' if Windows agent
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

        stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy to Local Docker Host') {
            steps {
                sh '''
                    docker rm -f my-web-app || true
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
