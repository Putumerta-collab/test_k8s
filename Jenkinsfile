pipeline {
    agent any

    environment {
        KUBE_CONFIG = credentials('kube-config') // Sesuaikan dengan nama credential kubeconfig kamu
    }
    stages {
            stage('Install kubectl') {
                steps {
                    sh '''
                        curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
                        chmod +x kubectl
                        mv kubectl /usr/local/bin/
                    '''
                }
            }
        
    stages {
        stage('Checkout') {
            steps {
                // Melakukan checkout dari repository GitHub
                git url: 'https://github.com/Putumerta-collab/test_k8s.git', branch: 'main'
            }
        }

        stage('Deploy NGINX') {
            steps {
                script {
                    // Mengaplikasikan deployment NGINX
                    sh 'kubectl apply -f k8s/nginx-deployment.yaml'
                    
                    // Mengaplikasikan service NGINX
                    sh 'kubectl apply -f k8s/nginx-service.yaml'
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment and Service applied successfully!'
        }
        failure {
            echo 'There was an issue with deploying NGINX!'
        }
    }
}
