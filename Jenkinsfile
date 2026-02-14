pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/santoshkmishra/Project1.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t santoshkmishra/static-site:latest .'
            }
        }

        stage('Push') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u santoshkmishra --password-stdin'
                    sh 'docker push santoshkmishra/static-site:latest'
                }
            }
        }

        stage('Deploy') {
            steps {
                // Stop and remove old container if exists
                sh '''
                docker ps -q --filter "name=static-site" | grep -q . && docker rm -f static-site || true
                docker run -d -p 8080:80 --name static-site santoshkmishra/static-site:latest
                '''
            }
        }
    }
}
