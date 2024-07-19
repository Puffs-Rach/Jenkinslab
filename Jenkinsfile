pipeline {
    agent any
    stages {
        stage('Init') {
            steps {
                script {
                    // Remove all containers (if any exist)
                    sh 'docker ps -qa | xargs -r docker rm -f || true'
                    // Create a Docker network if it doesn't exist
                    sh 'docker network create my-network || true'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Ensure requirements.txt is up-to-date with pinned versions
                    writeFile file: 'requirements.txt', text: '''\
Flask==2.0.2
Werkzeug==2.0.3
'''

                    // Build the Flask Docker image
                    sh 'docker build -t myapp .'
                }
            }
        }

        stage('Deploy MySQL') {
            steps {
                script {
                    // Run MySQL container with volume mounting
                    sh '''
                    docker run -d \
                        --network my-network \
                        --name mysql \
                        -e MYSQL_ROOT_PASSWORD=root \
                        -e MYSQL_DATABASE=mydb \
                        -v mysql-data:/var/lib/mysql \
                        mysql:latest
                    '''
                }
            }
        }

        stage('Deploy Flask App') {
            steps {
                script {
                    // Run the Flask Docker container
                    sh '''
                    docker run -d \
                        --network my-network \
                        -p 80:3000 \
                        --name myapp \
                        -e DB_HOST=mysql \
                        myapp:latest
                    '''
                }
            }
        }

        stage('Test Application') {
            steps {
                script {
                    // Wait for a few seconds to ensure the application is up
                    sh 'sleep 10'
                    // Test the application using curl
                    sh 'curl -v http://localhost || true'
                }
            }
        }
    }

    post {
        always {
            // Cleanup: remove the created Docker network
            sh 'docker network rm my-network || true'
        }
    }
}
