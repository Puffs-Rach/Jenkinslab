pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'echo "Building the project..."'
                sh 'ls -la'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Testing the project..."'
                sh 'pwd'
                sh 'touch testfile.txt'
                sh 'echo "This is a test file." > testfile.txt'
                sh 'ls -la'
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo "Deploying the project..."'
                sh 'mkdir -p deploy'
                sh 'mv testfile.txt deploy/'
                sh 'ls deploy'
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
