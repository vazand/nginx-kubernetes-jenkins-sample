pipeline {
    agent any

    stages {
        /* stage('Build Docker Image') {
            steps {
                sh 'docker build -t nginx-image .', label: 'Build Nginx Image'
            }
        }
        stage('Push Docker Image (Optional)') { // Uncomment if deploying to a remote registry
            when {
                expression { return env.PUSH_IMAGE == 'true' }
            }
            steps {
                sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                sh 'docker push nginx-image:latest'
            }
        } */
        
        stage('Deploy to Kubernetes') {
            
            steps {
                // check minikube cluster status
                echo "minikube status"
                // Apply deployment.yaml using kubectl
                script {
                    def process = sh(script: 'kubectl apply -f deployment.yaml', returnStdout: true)
                    if (process.exitCode != 0) {
                        error 'Deployment failed!'
                    }
                }
                // get the url of service app 
                echo "minikube service nginx-deployment --url"
            }
            
        }
    }

    post {
        always {
            // Add post-build actions here, e.g., send notifications
            echo 'Pipeline execution completed.'
        }
        success {
            // Actions on successful build (optional)
            echo 'Deployment successful!'
        }
        failure {
            // Actions on failed build
            echo 'Deployment failed!'
            // Send failure notification (optional)
        }
    }
}
