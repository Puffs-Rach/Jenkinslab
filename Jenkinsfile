pipeline {
    agent any
    stages {
        stage('Init') {
            steps {
                script {
                    sh '''
                        docker network create my-network || true
                        docker rm -f $(docker ps -qa) || true
                    '''
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    sh 'docker build -t myapp .'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh 'docker run -d -p 80:3000 --name myapp myapp:latest'
                }
            }
        }
    }
    post {
        always {
            script {
                sh 'docker ps -a'
            }
        }
    }
}
