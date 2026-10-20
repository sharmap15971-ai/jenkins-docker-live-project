pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t my-live-website .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker rm -f my-live-website || true
                    docker run -d \
                      --name my-live-website \
                      --restart unless-stopped \
                      -p 8081:80 \
                      my-live-website
                '''
            }
        }
    }
}