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
                    sh 'docker push $DOCKER_USER/static-site:latest'
                }
            }
        }
stage('Deploy') {
    steps {
        sh '''
        kubectl --kubeconfig=/home/jenkins/.kube/config apply -f k8s/deployment.yaml
        '''
    }
}
}
}
