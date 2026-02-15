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
        # Apply the deployment manifest
        kubectl --kubeconfig=/home/jenkins/.kube/config apply -f k8s/deployment.yaml

        # Force a rollout restart so pods pull the new image
        kubectl --kubeconfig=/home/jenkins/.kube/config rollout restart deployment/static-site

        # Wait until rollout completes
        kubectl --kubeconfig=/home/jenkins/.kube/config rollout status deployment/static-site

        # Show current pods for visibility
        kubectl --kubeconfig=/home/jenkins/.kube/config get pods -o wide

        # Show the service to confirm NodePort exposure
        kubectl --kubeconfig=/home/jenkins/.kube/config get svc static-site-service
        '''
    }
}
}
}
