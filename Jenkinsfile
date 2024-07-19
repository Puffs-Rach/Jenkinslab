pipeline {
    agent any
    stages {
        stage('Init') {
            steps {
                script {
                    // Remove all containers (if any exist)
                    sh 'docker ps -qa | xargs -r docker rm -f || true'
                    // Create a Docker network (optional, if needed)
                    sh 'docker network create my-network || true'
                }
            }
        }
        stage('Build') {
            steps {
                script {
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
    }
    post {
        always {
            // Clean up containers and networks if necessary
            sh 'docker ps -qa | xargs -r docker rm -f || true'
            sh 'docker network rm my-network || true'
        }
    }
}
