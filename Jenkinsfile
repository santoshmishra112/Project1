pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t santoshmishra92/static-site:latest .'
            }
        }
        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push $DOCKER_USER/static-site1:latest'
                }
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                docker ps -q --filter "name=static-site1" | grep -q . && docker rm -f static-site1 || true
                docker run -d -p 8081:80 --name static-site1 santoshmishra92/static-site1:latest
                '''
            }
        }
    }
}
