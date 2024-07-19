pipeline {

    agent any

    stages {

         stage('Init') {

            steps {

                sh 'docker rm -f $(docker ps -qa) || true'

            }

        }

        stage('Build') {

            steps {

                sh 'docker build -t myapp .'

            }

        }

 

        stage('Deploy') {

            steps {

                sh 'docker run -d -p 80:5500 --name myapp myapp:latest'

            }

        }

    }

}