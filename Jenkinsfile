pipeline {
    agent any
    
    environment {
        // Define environment variables here if needed
    }

    stages {
        stage('Checkout SCM') {
            steps {
                script {
                    // Checkout code from Git repository
                    checkout scm
                }
            }
        }

        stage('Init') {
            steps {
                script {
                    // Create Docker network (if needed)
                    sh 'docker network create my-network || true'
                    
                    // Remove all containers (optional, clean-up step)
                    sh 'docker ps -qa | xargs -r docker rm -f || true'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Update the requirements.txt to pin specific versions of Flask and Werkzeug
                    writeFile file: 'requirements.txt', text: '''\
Flask==2.0.2
Werkzeug==2.0.3
'''
                    
                    // Build the Docker image
                    sh 'docker build -t myapp .'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Run the Docker container
                    sh 'docker run -d -p 80:3000 --name myapp myapp:latest'
                }
            }
        }

        stage('Post Actions') {
            steps {
                script {
                    // Clean-up: remove the Docker network if needed
                    sh 'docker network rm my-network || true'
                    
                    // Clean-up: remove all containers
                    sh 'docker ps -qa | xargs -r docker rm -f || true'
                }
            }
        }
    }

    post {
        always {
            // Clean-up steps to ensure no residual resources
            sh 'docker ps -qa | xargs -r docker rm -f || true'
            sh 'docker network rm my-network || true'
        }
    }
}
