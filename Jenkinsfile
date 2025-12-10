pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo "Building the project..."
                sh 'ls -la'   // use bat 'dir' if running on Windows node
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                // sh './run-tests.sh'   // sample test command
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying..."
                // sh './deploy.sh'      // sample deploy step
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
